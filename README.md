

meta/unruh.cpp
// prime number computation
// (modified with permission from original from 1994 by Erwin Unruh) 

template<int p, int i>
struct is_prime {
   enum { pri = (p==2) || ((p%i) && is_prime<(i>2?p:0),i-1>::pri) };
};

template<>
struct is_prime<0,0> {
   enum {pri=1};
};

template<>
struct is_prime<0,1> {
   enum {pri=1};
};

template<int i>
struct D {
   D(void*); 
}; 

template<int i>
struct CondNull {
   static int const value = i;
};

template<>
struct CondNull<0> {
   static void* value;
};
void* CondNull<0>::value = 0;
template<int i>
struct Prime_print { 
          // primary template for loop to print prime numbers 
    Prime_print<i-1> a;
   enum { pri = is_prime<i,i-1>::pri };
   void f() {
     D<i> d =  CondNull<pri ? 1 : 0>::value;    
          // 1 is an error, 0 is fine
     a.f(); 
   } 
}; 

template<>
struct Prime_print<1> {       
          // full specialization to end the loop
  enum {pri=0};
  void f() {
     D<1> d = 0; 
   }; 
}; 

#ifndef LAST
#define LAST 18
#endif

int main() 
{ 
   Prime_print<LAST> a;
   a.f();
}
  
Produced the output:
unruh.cpp:39:14: error: no viable conversion from ’const int’ to ’D<17>’
unruh.cpp:39:14: error: no viable conversion from ’const int’ to ’D<13>’
unruh.cpp:39:14: error: no viable conversion from ’const int’ to ’D<11>’
unruh.cpp:39:14: error: no viable conversion from ’const int’ to ’D<7>’
unruh.cpp:39:14: error: no viable conversion from ’const int’ to ’D<5>’
unruh.cpp:39:14: error: no viable conversion from ’const int’ to ’D<3>’
unruh.cpp:39:14: error: no viable conversion from ’const int’ to ’D<2>’

"The earliest documented example of a metaprogram was by Erwin Unruh, then representing Siemens on the C++ standardization committee. He noted the computational completeness of the template instantiation process and demonstrated his point by developing the first metaprogram. He used the Metaware compiler and coaxed it into issuing error messages that would contain successive prime numbers. Here is the code that was circulated at a C++ committee meeting in 1994 (modified so that it now compiles on standard conforming compilers)" - David Vandevoorde, Douglas Gregor, Nicolai M. Josuttis, Introduction to metaprogramming in C++, infoworld, (2018), https://www.infoworld.com/article/3257727/introduction-to-metaprogramming-in-c-plus-plus.html
