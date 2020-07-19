---
layout: post
title: সি প্রোগ্রামের মেমোরি সেগমেন্টেশন
author: Saikat Sharma
tags: c, memory-management
---
সি তে মেমোরি সেগমেন্টেশন একটি গুরুত্বপুর্ণ পার্ট।  কম্পাইল করা একটি সি প্রোগ্রামের মেমোরি পাঁচটি ভাগে বিভক্ত হয়। 
- text 
- data
- bss
- heap
- stack

![Memory Segmentation](/assets/img/memory-segmentation.png)

**text segment:**

এই text segment কে code segment ও বলা হয়ে থাকে। এই সেগমেন্টে সাধারনত মেশিন ল্যান্গুয়েজ এর ইনস্ট্রাকশন গুলো থাকে। এই সেগমেন্টি read-only ও ফিক্স সাইজ।
```c
#include <stdio.h> 
int main(void) 
{ 
	return 0; 
}
```
উপরের কোডটি  রান করে   এর মেমোরি এর বিভিন্ন সেগমেন্ট এর সাইজ দেখতে পারি Unix এর size কমান্ড এর মাধ্যে।
```bash
$ gcc memory.c -o memory
$ size memory
   text	   data	    bss	    dec	    hex	filename
   1415	    544	      8	   1967	    7af	memory 
```
এখানে text হলো  text segment এর সাইজ, data হলো data segment এর সাইজ, আর bss হলো bss segment এর সাইজ,  dec  হলো ডেসিমেলে টোটাল সাইজ ও hex হলো হেক্সাডেসিমেলে টোটাল সাইজ। 

**data segment:** 

এই সেগমেন্টে ইনিসিয়ালাইজ করা global ও static ভ্যারিয়েবল সমুহ থাকে। 	
```c
#include <stdio.h>
int global_var = 10; 
int main(void) 
{   
    static int st_var = 10;
	return 0; 
}
```
উপরের কোডে আমরা  ইনিসিয়ালাইজ একটি ৪ বাইটের গ্লোবাল ইন্টিজার ও ৪ বাইটের একটি স্টাটিক ভ্যারিয়েবল ডিক্লেয়ার করেছি তাই নিচে মেমোরি সাইজে data এর ভ্যালু ৮ বাইট বেড়ে 544 থেকে বেড়ে 552 হয়ে গিয়েছে। 
```bash
$ gcc memory.c -o memory
$ size memory
   text	   data	    bss	    dec	    hex	filename
   1415	    552	      8	   1975	    7b7	memory
```

**bss segment:**

এই সেগমেন্টে আনইনিসিয়ালাইজ করা global ও static ভ্যারিয়েবল সমুহ থাকে।
```c
#include <stdio.h>
int global_var = 10;
int global_var_un;
int main(void)
{   
	static int st_var = 10;
	static int st_un;
	return 0;
}
```
উপরের কোডে আমরা আনইনিসিয়ালাইজ একটি ৪ বাইটের গ্লোবাল ইন্টিজার ও ৪ বাইটের একটি স্টাটিক ভ্যারিয়েবল ডিক্লেয়ার করেছি তাই নিচে মেমোরি সাইজে bss এর ভ্যালু ৮ বাইট বেড়ে 8 থেকে বেড়ে 16 হয়ে গিয়েছে। 
```bash
$ gcc memory.c -o memory
$ size memory
   text	   data	    bss	    dec	    hex	filename
   1415	    552	     16	   1975	    7b7	 memory
```
**heap segment:**

এই সেগমেন্টের মেমোরি প্রোগ্রামার কন্ট্রোল করতে পারে। অর্থাৎ প্রোগ্রামার এখানে  মেমোরি ব্লক এলোকেশন করতে পারে আবার ডিএলোকেশন করতে পারে। malloc, calloc দিয়ে সাধারনত এখানে মেমোরি এলোকেশন করা হয় এবং ডিএলোকেশন করা হয় free দিয়ে। এই সেগমেন্টের সাইজ  ফিক্সট নয়। 
```c
int main () {
    int *arr = (int *) malloc (sizeof (int) * 5);
    int *brr = (int *) calloc (5, sizeof (int) );
    free (arr);
    free (brr);
    return 0;
}
```
উপরের কোডে arr ও brr নামে দুইটি ৫ সাইজের অ্যারে heap segment টে এলোকেশন ও  ডিএলোকেশন করা হয়েছে। 

**stack segment:** 

লোকাল ভ্যারিয়েবল ও ফাংশন কল সমুহ এখানে স্টোর থাকে। এখানের সব তথ্য সমুহ একটি stack এ থাকে। আর এই stack কে অনেক গুলো stack frame থাকতে পারে। একটি ফাংশনের  যাবতীয় তথ্য সমুহ যেমন প্যারামিটার, লোকাল ভ্যারিয়েবল ও দুইটি পয়েন্টার saved frame pointer (SFP) এবং return address একটি stack frame এ থাকে।  ESP রেজিস্টারে stack এর শেষ এড্রেস্টি স্টোর থাকে। মেমোরি এড্রেসে এর এড্রেস উপর থেকে নিচের দিকে এসাইন হতে থাকে। 

### Reference
* [Memory Layout of C Programs](https://www.geeksforgeeks.org/memory-layout-of-c-program/)
* Book :- Hacking; the art of exploitation by jon erickson 
* Image from Hacking; the art of exploitation by jon erickson 

