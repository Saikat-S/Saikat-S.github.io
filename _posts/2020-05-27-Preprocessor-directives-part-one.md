### Preprocessor directives
Preprocessor directives সমুহ প্রিপ্রসেসর প্রোগ্রাম দ্বারা কম্পাইলেশনের আগে প্রসেস হয়।  নিচের ডায়াগ্রামটি দেখলে ব্যাপারটা একটু পরিষ্কার হবে। 

// diagram

সোর্স কোডটি কম্পাইলেশনের আগে চেক হয় কোন  প্রিপ্রসেসর ডিরেকটিভ আছে কিনা যদি থাকে তাহলে  প্রিপ্রসেসর প্রোগ্রাম দ্বারা সেটা প্রসেস হয় । তারপর কম্পাইল হয়ে অবজেক্ট ফাইল লিংকারের সাথে যুক্ত হয়ে এক্সিকেটেবল ফাইল হয়। 

Preprocessor directives গুলো # দিয়ে শুরু হয়।

**কিছু Preprocessor directives**
- macro definitions (#define, #undef)
- Conditional inclusions (#ifdef, #ifndef, #if, #endif, #else and #elif)
- Line control (#line)
- Error directive (#error)	
- Source file inclusion (#include)
- Pragma directive (#pragma)

#### macro definitions (#define, #undef)
[typedef](https://saikat-s.github.io/2020/05/25/What-is-typedef.html) এর মতো **define** দিয়েও টাইপ সমুহের ছোট ও সহজ **alias** অর্থাৎ **উপনাম** দেওয়া যায়। তবে **define** দিয়ে শুধু টাইপ না **কনস্টেন্ট, ফাংশন, স্টেটমেন্টে, এক্সপ্রেশন, ব্লক ও অন্য যে কোন কিছুর উপনাম** দেওয়া যায়। এই উপনাম সমুহকেই **macro** বলা হয়।  

**সিনটেক্স ডিক্লারেশন:**
```c
#define identifier replacement
```

প্রিপ্রসেসরের পর  identifier replacement  দিয়ে পরিবর্তিত হয়ে যাবে। যেমন, 

```c
#define TABLE_SIZE 100
int table1[TABLE_SIZE];
int table2[TABLE_SIZE];
```
প্রিপ্রসেসরের পর উপরের কোডটি নিচের মতো হয়ে যাবে। 
```c
int table1[100];
int table2[100]; 
```
এক কথায়  define  করা macro সমুহ প্রিপ্রসেসরের  পর আসল ভেলু দিয়ে পরিবর্তিত হয়ে যায়। 
```c
#define WIDTH   80
#define LENGTH  ( WIDTH + 10 )
int main(){
    int var = LENGTH * 20;
}
```
প্রিপ্রসেসরের পর হবে,
```c
int main(){
    int var = ( 80 + 10 ) * 20;
}
```
এবং **var** এর ভ্যালু হবে **1800**। কিন্তু ডিফাইনে **ব্যাকেট** না দিলে কোডটি হবে, 
```c
int main(){
    int var =  80 + 10  * 20;
}
```
এবং **var** এর ভ্যালু হবে **280**।
```c
#define NAME "github"
int main(){
    char ch[7] = NAME;
}
```
প্রিপ্রসেসরের পর উপরের কোডটি হবে নিচের মতো। 
```c
int main(){
    char ch[7] = "github";
}
```
**Function macros:** 
**define** দিয়ে প্যারামিটার পাস করেও **macro** ডিক্লেয়ার করা যায়। একে **Function macro definitions** বলা হয়। 

**সিনটেক্স ডিক্লারেশন:**
```c
#define identifier ( identifieropt, ... , identifieropt ) replacement
```
যেমন, 
```c
#define getmax(a,b) a>b?a:b 
```
```c
// function macro
#define getmax(a,b) ((a)>(b)?(a):(b))
int main()
{
  int x = 5, y;
  y = getmax(x,2);       
  printf("%d\n", y);    // output = 5
  printf("%d\n", getmax(7,y)); // output = 7
  return 0;
}
```

**#undef**
আমরা কোন **macro** কে **ডিফাইন** করার পর **আনডিফাইন** না করা পর্যন্ত তা প্রোগ্রামে থেকেই যায়। এবং আমরা সেই macro এর ভেলু পরিবর্তন করতে পারি না। যেমন, 

```c
#define MAX 10005
int main(){
	int arr[MAX];
	MAX = 200005;
	return 0;
}
```

আমরা যদি উপরের কোড টি রান করি তাহলে ইরর পাবো। কারণ আমরা **MAX macro** কে  আনডিফাইন না করেই পাল্টানোর চেস্টা করেছি। এই সমস্যা সমাধানের জন্য প্রথমে আমাদের ডিফাইন করা macro কে **আনডিফাইন** করতে হবে নিচের উপায়ে, 
```c
#undef macro_name
```
আনডিফাইন করার পর আমরা আবার নতুন করে **macro** এর নাম টি ব্যবহার করতে পারবো। 
```c
#define MAX 10005
int main(){
	int arr[MAX];
	#undef MAX
    int MAX = 200005;
	return 0;
}
```

উপরের কোডটি এখন কোন ইরর ছাড়াই রান করবে।

**loop কে আমরা নিচের উপায়ে ডিফাইন করতে পারি**।
```c
#define FOR(i, N) for(int i = 0; i<N; i++)
int main(){
	int N = 5;
   	 FOR(i,N){
		printf(“%d\n”, i);
	}
}
```
প্রিপ্রসেসরের পর উপরের কোডটি নিচের মতো হয়ে যাবে।
```c
int main(){
    int N = 5;
    for (int i = 0; i < N; i++) {
        printf(“%d\n”, i);
    }
}
```
আমরা আমাদের নিজেদের সুবিধা মতো প্যারামিটার ঠিক করে নিজের জন্য macro ডিফাইন করতে পারি। 

**multiple lines macros**
শেষ লাইন বাদে বাকি লাইন গুলোর শেষে **ব্যাকস্ল্যাশ ( \ )** চিহ্ন দিয়ে **মাল্টিলাইন macro** ডিফাইন করা যায়। 
```c
#define MULTILINE(i, N) for(int i = 0; i<N; i++) \ 
                        { \ 
			                printf("%d\n", i);\
                        } 
int main(){
	int N = 5;
   	MULTILINE(i,N);
}
```



**Preprocessor operators**
**Stringizing operator (#)**
**Function macro** ডেফিনেশনে # ওপারেটর টি # এর পরে যে টোকেনটি পায় তাকে **স্ট্রিং** এ পরিনত  করে দেয়। 
```c
#define STR(x) #x
int main(){
    printf("%s\n", STR(hello));
	printf("%s\n", STR("hello"));
}
```
```c
// output
hello
"hello"
```
**Token-pasting operator (##)**
**Function macro** ডেফিনেশনে ## ওপারেটর টি ## এর আগে ও পরে যে টোকেন দুটি পায় সে  টোকেন দুটি কে **জোড়া** লাগিয়ে দেয়। 
```c
#define MERGE(a, b) a##b
int main(){
	 int c = MERGE(12,345);
	 printf("%d\n", c);
}
```
```c
// output
12345
```




#### References
* [Preprocessor directives](http://www.cplusplus.com/doc/tutorial/preprocessor/)
* [C/C++ Preprocessors](https://www.geeksforgeeks.org/cc-preprocessors/)
* [Preprocessor operators](https://docs.microsoft.com/en-us/cpp/preprocessor/preprocessor-operators?view=vs-2019)
* [#define directive (C/C++)](https://docs.microsoft.com/en-us/cpp/preprocessor/hash-define-directive-c-cpp?view=vs-2019)

