---
layout: post
title: সি প্রোগ্রামের ভ্যারিয়েডিক ফাংশন
author: Saikat Sharma
tags: c, function, variadic_function
---

**Variadic Functions**

সি প্রোগ্রামিং ল্যাঙ্গুয়েজে ফাংশন ডিক্লারেশনের সময় আমাদের ফাংশন এর আর্গুমেন্ট সংখ্যা ও   আর্গুমেন্ট টাইপ ঠিক করে বলে দিতে হয়। কিন্তু ধরুন আমরা যদি এমন একটা ফাংশন পেতাম যেটা আপনার ইচ্ছা মতো  আর্গুমেন্ট সংখ্যা নিবে। অর্থাৎ একই ফাংশন ডেফিনেশনে আপনি চাইলে একটা, দুইটা, পাঁচটা এমন কি ইচ্ছা মতো  আর্গুমেন্ট দিতে পারেন। হ্যা এইভাবেও ফাংশন ডিক্লেয়ার করা যায় আর এই  ফাংশন ডিক্লারেশনকেই বলে ভ্যারিয়েডিক ফাংশন (variadic function )। 

অর্থাৎ যে ফাংশনের আর্গুমেন্ট সংখ্যা নির্ধারিত নয় বরং ভ্যারিয়েবল সেটিই  ভ্যারিয়েডিক ফাংশন (variadic function )।

**ভ্যারিয়েডিক ফাংশন এর ডিক্লারেশন ও ব্যবহার:**

ভ্যারিয়েডিক ফাংশন ব্যবহার করতে নিম্নোক্ত  টাইপ ও ম্যাক্রোস গুলো দরকার হয়। 

টাইপ: va_list

ম্যাক্রোস: va_start , va_arg, va_copy, va_end

উপরোক্ত  টাইপ ও ম্যাক্রোস সমুহ stdarg.h হেডার ফাইলের অন্তর্ভুক্ত। 

**ভ্যারিয়েডিক ফাংশন সিনটেক্স ডিক্লারেশন:**
```c
return_type function_name(type  last-required, …);
```
return_type হলো ফাংশনের রিটার্ন টাইপ,  last-required হলো  আর্গুমেন্ট সংখ্যা ও টাইপ সম্পর্কে তথ্য। আর (…)  অর্থাৎ তিনটি ডট কে বলা হয় ellipsis. এটা দিয়েই বোঝানো হয় এটা ভ্যারিয়েডিক ফাংশন। 

**va_list**

এটা  আর্গুমেন্ট পয়েন্টার ভ্যারিয়েবল হিসেবে কাজ করে।  আর্গুমেন্ট এর পয়েন্টার দিয়ে এর ভ্যালু এক্সেস করা হয়। 

**সিনটেক্স ডিক্লারেশন:**
```c
va_list “argument pointer variables name”;  // syntex
va_list ap; // ap is  argument pointer variables name
```
**va_start**

ভ্যারিয়েডিক ফাংশন এর আর্গুমেন্ট পয়েন্টার ইনিশিয়ালাইজ করা হয় va_start  দিয়ে।

**সিনটেক্স ডিক্লারেশন:**
```c
void va_start (va_list ap, last-required);
```
এখানে ap হলো ভ্যারিয়েডিক ফাংশন এর আর্গুমেন্ট পয়েন্টার যার টাইপ হলো va_list  এবং  last-required হলো  ভ্যারিয়েডিক ফাংশন ডিক্লারেশনের প্রথম  আর্গুমেন্ট। 

**va_arg** 

পরের  ভ্যারিয়েডিক ফাংশন এর  আর্গুমেন্ট পযেন্টার ও আর্গুমেন্টের ভ্যালু এক্সেস করা হয় va_arg দিয়ে । 

**সিনটেক্স ডিক্লারেশন:**
```c
type va_arg (va_list ap, type)
```
এখানে ap হলো ভ্যারিয়েডিক ফাংশন এর আর্গুমেন্ট পয়েন্টার যার টাইপ হলো va_list  এবং type হলো  আর্গুমেন্ট এর ডাটা টাইপ। 

**va_copy**

এক ভ্যারিয়েডিক ফাংশন আর্গুমেন্টকে আরেক ভ্যারিয়েডিক ফাংশন আর্গুমেন্টে কপি করা হয় va_copy দিয়ে।

**সিনটেক্স ডিক্লারেশন:**
```c
void va_copy (va_list dest, va_list src)
```

**va_end**

ভ্যারিয়েডিক ফাংশন আর্গুমেন্ট va_end এর মাধ্যমে ক্লোজ করা হয়। 

**সিনটেক্স ডিক্লারেশন:**
```c
void va_end (va_list ap)
```


**উদাহরণ:**
```c
#include <stdio.h>
#include <stdarg.h>

int sum_all (int n, ...) {
    va_list ap; // ap is the argument pointer variable
    va_start (ap, n); // initialize ap
    int sum = 0;
    for (int i = 0; i < n; i++) {
        int val = va_arg (ap, int); // access the argument value
        sum += val;
    }
    va_end (ap); // ends traversal of the variadic function arguments 
    return sum;
}
int main() {
    int sum = sum_all (3, 1, 2, 3);
    printf ("%d\n", sum);
    sum = sum_all (5, 1, 2, 3, 4, 5);
    printf ("%d\n", sum);
    sum = sum_all (7, 5, 1, 2, 3, 4, 5, 9);
    printf ("%d\n", sum);
    return 0;
}
```
```c
output:
6
15
29
```
এখানে প্রথমে va_list দিয়ে ap নামে একটি আর্গুমেন্ট পয়েন্টার ভ্যারিয়েবল ডিক্লেয়ার করা হয়েছে।  va_start দিয়ে  ap কে বর্তমান ফাংশন এর প্রথম আর্গুমেন্টের পয়েন্টার ও আর্গুমেন্ট সংখ্যা  দিয়ে  ইনিশিয়ালাইজ করা হয়েছে। বর্তমান আর্গুমেন্ট পয়েন্টার ap ও এর ডাটা টাইপ দিয়ে va_arg  এর মাধ্যমে   আর্গুমেন্ট ভালুটি এক্সেস করা হয়েছে। ঠিক এটা কল হওয়ার পর ap এর ভ্যালু হয়ে যায় পরের  আর্গুমেন্ট পয়েন্টার। এবং শেষে   va_end  দিয়ে ap কে ক্লোজ করে দেওয়া হয়।

এই প্রোগ্রামে  last-required হিসেবে আর্গুমেন্ট সংখ্যা পাস করানো হয়েছে অর্থাৎ ফাংশনটিকে এখন কয়টি আর্গুমেন্ট নিয়ে কাজ করতে হবে তার সংখ্যাটি বলে দেওয়া হয়েছে।  নিচে  last-required হিসেবে আর্গুমেন্ট এর টাইপ ও সংখ্যা উভয় নিয়ে কাজ করার একটি প্রোগ্রাম দেওয়া হলো।  

```c
#include <stdio.h>
#include <stdarg.h>
 
void simple_printf(const char* fmt, ...){
    va_list args;
    va_start(args, fmt);
 
    while (*fmt != '\0') {
        if (*fmt == 'd') {
            int i = va_arg(args, int);
            printf("%d\n", i);
        } else if (*fmt == 'c') {
            // A 'char' variable will be promoted to 'int'
            // A character literal in C is already 'int' by itself
            int c = va_arg(args, int);
            printf("%c\n", c);
        } else if (*fmt == 'f') {
            double d = va_arg(args, double);
            printf("%f\n", d);
        }
        ++fmt;
    }
    va_end(args);
}
int main(void){
    simple_printf("dcff", 3, 's', 3.1516, 42.5); 
}
```
```c
output
3
s
3.151600
42.500000
```
এখানে last-require হিসেবে ক্যারেক্টার অ্যারে পাঠানো হয়েছে যা আর্গুমেন্ট এর টাইপ সমুহ ধারন করে। 



### Problems
* [Variadic functions in C - HackerRank](https://www.hackerrank.com/challenges/variadic-functions-in-c/problem?h_r=profile)

### Reference
* [Variadic Functions - gnu.org](https://www.gnu.org/software/libc/manual/html_node/Variadic-Functions.html)
* [Variadic Functions - cppreference](https://en.cppreference.com/w/c/variadic)

