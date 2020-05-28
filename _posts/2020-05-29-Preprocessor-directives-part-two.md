---
layout: post
title: Preprocessor directives দ্বিতীয় পার্ট 
author: Saikat Sharma
tags: c, c++, preprocessor directives
---

[Preprocessor directives প্রথম পার্ট](https://saikat-s.github.io/2020/05/27/Preprocessor-directives-part-one.html)

#### Conditional inclusions (#ifdef, #ifndef, #if, #endif, #else and #elif)
**#ifdef**
**সিনটেক্স ডিক্লারেশন:**
```c
#ifdef macro_name
    statement1;
    ........;
    statementN;
#enfif
```

যদি **macro_name** আগে ডিফাইন করা থাকে তাহলে **#ifdef** এর ভেতরে ডুকবে। আর **macro_name** আনডিফাইন হলে **#ifdef**  কে **স্কিপ** করে যাবে।  যেমন, 


```c
int main(){
    #ifdef NAME
        printf("NAME is defined.\n");
    #endif
  return 0;
}
```
```c
// output

```

উপরের কোড কিছুই **আউটপুট** দিবে না কারণ **NAME macro** টি ডিফাইন করা নেই। কিন্তু নিচের কোডটি আউটপুট দিবে কারণ  **NAME macro** টি ডিফাইন করা আছে। 

```c
#define NAME "anything";
int main(){
    #ifdef NAME
	    printf("NAME is defined.\n");
    #endif
  return 0;
}
```
```c
// output
NAME is defined.
```

**#ifndef**

**সিনটেক্স ডিক্লারেশন:**
```c
#ifdef macro_name
    statement1;
    ........;
    statementN;
#enfif
```
এটা **#ifdef** এর উল্টা এখানে যদি  **macro_name** **ডিফাইন** করা না থাকে তাহলে **#ifndef** এর ভেতরে ডুকবে। আর **macro_name**  ডিফাইন করা থাকলে **#ifndef**  কে **স্কিপ** করে যাবে।  যেমন, 

```c
#define NAME "anything";
int main(){
 #ifndef NAME
	printf("NAME is not defined.\n");
  #endif
  return 0;
}
```
```c
// output

```

উপরের কোড কিছুই **আউটপুট** দিবে না কারণ **NAME macro** টি ডিফাইন করা আছে। কিন্তু নিচের কোডটি **আউটপুট** দিবে কারণ  **NAME macro** টি  ডিফাইন করা নেই। 

```c
int main(){
 #ifdef NAME
	printf("NAME is not defined.\n");
  #endif
  return 0;
}
```
```c
// output
NAME is not defined.
```

**#if, #elifand #else**

এটা অনেক টা সাধারণ **if-else** এর মতোই কাজ করে। নিচের কোডটি দেখুন, 
```c
#if TABLE_SIZE>200
#undef TABLE_SIZE
#define TABLE_SIZE 200

#elif TABLE_SIZE<50
#undef TABLE_SIZE
#define TABLE_SIZE 50
 
#else
#undef TABLE_SIZE
#define TABLE_SIZE 100
#endif

int table[TABLE_SIZE]; 
```

উপরের কোডে যখন **TABLE_SIZE 200** এর বেশি হবে তখন **TABLE_SIZE** কে **রিডিফাইন** করে **200** করা হচ্ছে কিন্তু যদি **TABLE_SIZE 50** এর থেকে কম হয় তাহলে  **TABLE_SIZE**  **রিডিফাইন** হয়ে **50** হচ্ছে আর এই দুই কন্ডিশন না মিললে **TABLE_SIZE**  **100** করে দেওয়া হচ্ছে। 

```c
#if defined ARRAY_SIZE
#define TABLE_SIZE ARRAY_SIZE
#elif !defined BUFFER_SIZE
#define TABLE_SIZE 128
#else
#define TABLE_SIZE BUFFER_SIZE
#endif 
```

**Line control (#line)**

আমরা যখন কোন প্রোগ্রাম কম্পাইল করে ভুল পাই তখন কম্পাইলার আমাদের কোন ফাইলে ভুল এবং   কতো নম্বর লাইনে ভুল হয়েছে সেটা বলে দেয়। **#line** দিয়ে আমরা ঠিক সেই কাজটিই করতে পারি। 
 
**সিনটেক্স ডিক্লারেশন:**
```c
#line number "filename"
```
এখানে **number** হলো লাইন নম্বর যেটা **#line** ডিফাইনের পরের লাইন থেকে  লাইন প্রতি  এক করে বাড়তে থাকে। আর  **"filename"** হলো ফাইলের নাম অথবা কি ধরনের ভুল সেটাও দিতে পারি তবে এটা **বাদ্ধতামুলক** নয়। 
```c
int man(){
	#line 20 "assigning variable"
	int a;
	int b;
	int c;
	int d?;
	return 0;
}
```
উপরে কোডটি কম্পাইল করলে নিচের মতো একটি **ইরর** পাবেন। 
```c
assigning variable:23:7: error: expected initializer before ‘?’ token
```
কারণ আমরা যেহেতু  **number  = 20** দিয়েছি তাই তিন লাইন পর  **23** নম্বর লাইনে  **int d?** ডিক্লারেশনে ভুল ধরেছে।  

**Error directive (#error)**

 কোন **macro** ডিফাইন করা না থাকলে কম্পাইলেশন টাইমে  **ইরর** প্রকাশ করানো যায় **#error** দিযে।।
```c
#define NAME "true";
int main(){
	#ifndef NAME
	    #error NAME is not defined!
	#endif 
	return 0;
}
```
উপরের কোডে কোন **ইরর** দিবে না। কিন্তু নিচের কোডে **ইরর** দিবে। কারণ নিচের কোডে **NAME** ডিফাইন করা নেই। 
```c
int main(){
	#ifndef NAME
	    #error NAME is not defined!
	#endif 
	return 0;
}
```
```c
// error
#error NAME is not defined!
```

**Source file inclusion (#include)**

**#include** দিয়ে আমরা প্রোগ্রামে হেডার ফাইল যুক্ত করে থাকি। 

**সিনটেক্স ডিক্লারেশন:**
```c
#include <header>   // build-in 
#include "file"     // user define
```
**Pragma directive (#pragma)**

**Pragma directive** সমুহ কম্পাইলারের বিশেষ কিছু ফিচার ব্যবহার করার সুযোগ করে দেয় **যা কম্পাইলার ভেদে আলাদা হয়**। কোন কম্পাইলে যদি কোন  ফিচার  না থাকে আর প্রোগ্রামে তা লিখা লাগে তাহলেও সেই কম্পাইলার  কোন ইরর দেয় না কারণ **কম্পাইলার তা ইগনোর করে থাকে**। 

#### Predefined macro names

|macro নাম      |   macro আউটপুট    |
| --- | --- |
| **__LINE __** | এটা প্রোগ্রামটির বর্তমান লাইন নম্বর দিবে ইন্টিজার হিসেবে। |
| **__FILE __** | এটা প্রোগ্রামটির ফাইল নাম  দিবে স্ট্রিং হিসেবে।|
| **__DATE __**  | এটা প্রোগ্রামটির কম্পাইলেশনের তারিখ দিবে স্ট্রিং হিসেবে "Mmm dd yyyy" ফর্মেটে। |
| **__TIME __** | এটা প্রোগ্রামটির কম্পাইলেশনের সময় দিবে স্ট্রিং হিসেবে "hh:mm:ss" ফর্মেটে। |
| **__cplusplus** | এটা একটা লং ইন্টিজার দিবে কম্পাইলার এর ভারশন অনুসারে। 199711L: ISO C++ 1998/2003  201103L: ISO C++ 2011 |

```c
int main(){
	printf("This is the line number %d", __LINE__);
	printf(" of file %s.\n", __FILE__);
	printf("Its compilation began %s", __DATE__);
	printf(" at %s\n", __TIME__);
	printf("Compiler gives a __cplusplus value of %ld\n",__cplusplus);
	return 0;
}
```
উপরের কোড রান করলে নিচের মতো **আউটপুট** আসবে। 
```c
This is the line number 124 of file preprocessor_directives.cpp.
Its compilation began May 27 2020 at 21:42:57
The compiler gives a __cplusplus value of 201103
```

#### References
* [Preprocessor directives](http://www.cplusplus.com/doc/tutorial/preprocessor/)
* [C/C++ Preprocessors](https://www.geeksforgeeks.org/cc-preprocessors/)
* [Preprocessor operators](https://docs.microsoft.com/en-us/cpp/preprocessor/preprocessor-operators?view=vs-2019)
* [#define directive (C/C++)](https://docs.microsoft.com/en-us/cpp/preprocessor/hash-define-directive-c-cpp?view=vs-2019)

