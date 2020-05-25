---
layout: post
title: typedef কথন
author: Saikat Sharma
tags: c, c++, typedef
---

### typedef 
**typedef** হলো **C/C++** এর এমন একটা **keyword** যেটা বিদ্যমান কোন ডাটা-টাইপের **উপনাম** দিতে পারে। মানে ধরুন আমাদের একটা ডাটা-টাইপ আছে **unsigned int** এটা একটু  বড় তাই আমরা চাইলে এটার একটা ছোট সহজ নাম দিতে পারি যেমন **uint**. 

**সিনটেক্স ডিক্লারেশন:**
```c
typedef current_name new_name;
```
এখন আমরা  **unsigned int** কে **typedef** করতে পারি নিচের উপায়ে, 
```c
typedef unsigned int uint;
```
এখন  
```c
unsigned int variables1;
uint variables2;
```
এখানে **variables1** ও **variables2** একই কাজ করবে। সাধারনত একটু কমপ্লেক্স টাইপ গুলোকে typedef এর মাধ্যমে সহজ করে নেয়া হয়।
যেমন, 
```c
struct student
{
    char name[30];
    int id;
};
```
উপরের **student** নামক **structure** কে ডিক্লার করতে হবে আমাদের নিচের উপায়ে,
```c
struct student student1;
```
কিন্তু আমরা  typedef এর মাধ্যমে এটাকে ছোট ও সহজ করে দিতে পারি নিচের উপায়ে,
```c
typedef struct student st;
st student2;
```
এটাকে সরাসরি নিচের উপায়েও ডিক্লার করা যায়।
```c
typedef struct student
{
    char name[30];
    int id;
}st;
```

#### এক typedef এ একাধিক ডিক্লারেশন 
একই ডাটা-টাইপের অনেক ভেরিয়েবল ডিক্লার করা যায় এক লাইনে। যেমন,
 ```c
 int val1, val2;
 ```
তেমনি একটি  typedef দিয়ে একই ডাটাটাইপ এর বিভিন্ন  টাইপ typedef করা যায়।
```c
typedef int int_t;              // simple int
typedef int *intp_t;            // pointer to int
typedef int (&fp)(int, int);    // reference to function returning int
typedef int arr_t[10];          // array of 10 ints
```
এখানে **int** ডাটা-টাইপের বিভিন্ন typedef করা হয়েছে। আমরা এই সব typedef কে এক  typedef দিয়ে ডিক্লার করতে পারি নিচের উপায়,
```c
typedef int int_t, *intp_t, (&fp)(int,int), arr_t[10];
```
এখানে, 
- **int_t** হলো int এর সাধারন **typedef**.
- ***int_t** হলো **pointer typedef**.
- **(&fp)(int, int)**  হলো  **reference to a function typedef** যার রিটার্ন টাইপ হলো **int** এরং দুটো int টাইপ প্যারামিটার।
- **arr_t[10]** হলো ১০ সাইজে **array** ডিক্লার **typedef**.

**Pointer typedef ডিক্লারেশন:**
```c
typedef struct {
    int a, b;
} S, *pS;
```
```c
// the following two objects have the same type
pS ps1;
S* ps2;
```

**arr_t[10] ডিক্লারেশন:**
```c
// the following two objects have the same type
int ar1[10];
arr_t ar2;
```

#### What does (&fp)(int, int) means?
**(&fp)(int, int)** দিয়ে বুঝায় যে **fp** একটি **function pointer** যেটা এমন একটা function কে refere করে যার int টাইপের দুটো প্যারামিটার আছে এবং যার রিটার্ন টাইপ int.
নিচের কোডটি দেখলে বুঝতে পারবেন।
```c
typedef int (&fp)(int, int);
int fun(int param1, int param2) {
      int x = param1 + param2;
     return x;
}
int main() {
     fp fp_function = fun;
     int x = fp_function(0, 1);
     return 0;
}
```

#### References
* [typedef specifier](https://en.cppreference.com/w/cpp/language/typedef)
* [What does this typedef statement mean?](https://stackoverflow.com/questions/22061750/what-does-this-typedef-statement-mean)
