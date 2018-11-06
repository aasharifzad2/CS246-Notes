# CS246 Notes

## Lecture 5 - Testing, C++ Intro

### Testing

- Testing is not the same as debugging.
- Testing doies not guarantee correctness
  - Can prove presence of defects

#### Writing Tests

- Write small test cases

- Check various classes of input

  - Numeric ranges: positive, negative, zero
  - Edge cases: Boundaries of ranges
  - Corner cases: Simultaneous boundaries



### Introduction to C++

- Simula67 was the first OO language, but it wasn't used very much.
- Bjarne Stroustrup added OO features to C, calling it *C with classes* which then became C++
- We will be using C++14



#### Hello World in C++

```c++
#include <iostream>
using namespace std;

int main ()
{
  cout << "Hello world" << endl;
  return 0;
}
```



#### Compiling C++ Programs

We are going to use `g++`  for compiling

To do this we execute `$> g++ -std=c++14 hello.cc -o execname`

- The `-o` is for setting the name of the executable that gets created. Without it the executable file output is defaulted to be `a.out`.
- We created an alias in the linux remote environment `g++14` so that replaces `g++ -std=c++14`



#### C++ I/O

Including iostream gives us access to the 3 I/O streams

1. `std::cin`

   - Purpose: reads from `stdin`
   - Use the input operator `>>`
   - e.g. `std:cin >> x`

2. `std::cout`

   - Purpose: writes to `stdout`
   - Use the output operator `<<`
   - e.g. `std::cout << x`

3. `std::cerr`

   - Purpose: writes to `stderr`
   - Use the output operator `<<`
   - e.g. `std::cerr << "ERROR"`

##### Example: Adding two numbers

```c++
#include <iostream>
using namespace std;

int main ()
{
  int x, y;
  cin >> x >> y;
  cout << x + y << endl;
}
```

##### Read Fails

- `cin` will read from the first non-whitespace character until it hits whitespace.
- It then ignores whitespace when reading ints
- If a read fails due to bad input type, variable is set to 0
- If a read fails due to no more input (`EOF`) var is set to 0
- If a read fails, the expression `cin.fail()` is true
- If the read fails due to `EOF`, bothe `cin.eof()` will be true as well

##### Example: Read all ints and echo them

```c++
#include <iostream>
using namespace std;

int main() {
  int i;
  while (true) {
    cin >> i;
    if (cin.fail()) break;
    cout << i << endl;
  }
}
```

C++ defines an automatic conversion from an `istream` type to a `boolean`

`cin` has type `instream` so `cin` is true if the last read was successful. Thus we can use `!cin` instead of `cin.fail()`.

```c++
#include <iostream>
using namespace std;

int main()
{
  int i;
  while (true) {
    cin >> i;
    if (!cin) break;
    cout << i << endl;
  }
}
```



##### Bitshift Operator

```c++
int x = 5;
x >> 3;  // right bit shift
```

In c++, >> can act as botht the *bit shift* or input operator.

`cin >> i` will produce cin

`cin >> x >> y;` -> `cin >> y;` -> `cin;`

ReadInts3

```c++
#include <iostream>
using namespace std;

int main()
{
  int i;
  while (true) {
    if (!(cin >> i)) break;
    cout << i << endl;
  }
}
```

ReadInts4

```c++
#include <iostream>
using namespace std;

int main() {
  int i;
  while (cin >> i) {
    cout << i << endl;
  }
}
```

##### Example: Read ints and echo them to stdout until EOF. Skip non-int input.

```c++
#include <iostream>
using namespace std;

int main ()
{
  int i;
  while (true)
  {
    if (!(cin >> i))
    {
      if (cin.eof()) break;
      else {
        cin.clear();
        cin.ignore();
      }
    }
    else
    {
      cout << i << endl;
    }
  }
}
```

If a read fails, all subsequent reads also fail unless the fail is acknowledged. Use `cin.clear()`

If the read fails and isnt eof, we need to throw away the bad input using `cin.ignore()`. This ignores one character.



## Lecture 6 - More I/O; Streams

### Strings in C++

C used null terminated char arrays

- manual memory managment

C++ provides a `<string>` header

- Automatic resizing as needed

`string s = "hello";`

`string s` is a C++ string, while `"hello"` us a C-style string, so some C++ magic is going on

#### Comparisons

In C, we used stringcmp

In C++ we can use `==, !=, <, >, <= & >=` since the strings are no longer pointers

#### Length

C		 `strlen`

C++	 `str.length()`

#### Characters

C		`str[0], str[1]`

C++		`str[0], str[1]`

#### Concatenation

C		`strcat(str1, str2)` (had to make sure str1 had enough space, str1 was overwritten)

C++		`str3 = str1 + str2`

#### Reading Strings

```c++
#include <iostream>
#include <string>
using namespace std;

int main () {
  string s;
  cin >> s;
  cout << s << endl;
}
```

- `cin` reads from first non-whitespace char to the first whitespace char
- to read the entire line use `getline(cin, s)` which reads everything from `stdin` until a newline character

#### Format Specifiers

##### C

`scanf` with `%d, %s`

`printf` with `%s, %d, %x`

##### C++

`int x = 95`

`cout << x; // prints in decimal`

##### C++ I/O Manipulators

iostream, iomanip

Note that some manipulators are sticky (meaning the setting will persist after printing), and some are non-sticky (meaning the setting reverts back to the original after)

###### Print `x` in hexadecimal

`cout << hex << x`

###### Change `cout` back to decimal output

`cout << dec`

###### Print with decimal point to 3 places

`cout << showpoint << setprecision(3);`

###### Print true instead of 1, and false instead of 0

`boolalpha` **TODO: Look up**



### Streams

The "stream abstraction" can work on other sources of data

#### Reading & writing files

##### The `<fstream>` Header

`ifstream`	read from files

`ofstream`	write to files

```c++
#include <iostream>
#include <fstream>
using namespace std;

int main ()
{
  ifstream myfile{"myfile.txt"};
  string s;
  while (myfile >> s)
  {
    cout << s << endl;
  }
}
```

Anything we can do with `cin` (type `istream`) we can do with `myfile` (type `ifstream`). And anything we can do with `cout` (type `ostream`) with a variable of type `ofstream`.

###### When is this this file closed?

myfile is stack allocated, so it is destroyed when it goes out of scope

when it is destroyed, it closes the file



#### Can read/write to strings

header sstream

`istringstream` - read from strings

`ostringstream` - write to strings

```c++
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

int main ()
{
  ostringstream oss;
  int lo {1}, hi {100};
  oss << "Enter a # between " << lo << " and " << hi;
  string s {oss.str()};
  cout << s << endl;
}
```

###### Insist user inputs a number

```c++
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

int main ()
{
  int n;
  while (true)
  {
    cout << "Enter a number:" << endl;
    string s;
    cin >> s;
    istringstream ss{s};
    if (ss >> n) break;
    cout << "I said, ";
  }
  cout << "You entered " << n << endl;
}
```

### New in C++: Default arguments

```c++
void printFile(string file = "myfile.txt")
{
    ifstream f{file};
}

printFile(); // defaults to 'myfile.txt'
printFile("hello.txt");
```

Parameters with default arguments must appear last, so you cannot have: `foo(int x = 5, string y){}`.

If you have: `foo(int x = 0, string y = ""){}`

You can call: `foo(); foo(9); foo(9, "apples");` but not `foo("apples"); foo(,"apples");`

### Function Overloading

```
int negate(int i) { return -i; }
bool negate(bool b) {return !b; }
```

C allows functions to be created with the same name, as long as their paramaters differ.

**TODO:** you have foo(int x, int y = 4); can you implement foo(int x)

Operator overloading is an example of function overloading

## Lecture 7

### Structs

###### C

```C
struct Node
{
	int data;
	struct Node *next;
}

struct Node n = {3, NULL}
```

###### C++

```C++
struct Node
{
    int data;
    Node *next;
}

Node n = {3l, nullptr}
```

### Constants

```c++
const int MAX = 100;
const Node n1{5, nullptr}; //cannot change fields of n1
int x = 10;

// p is a pointer to an int that's constant
// can do p=&y
// cannot do *p=5
const int *p=&x;

// q is a constant pointer to an int
// cannot do q=&y;
// can do *q=10;
int * const q=&x;
```

### Parameter Passing

Default: pass by value

```C++
//PASS BY VALUE
void inc(int n)
{
    n= n+1
}
...
int x = 5;
inc(x);
cout << x; // Prints 5
```

#### Pass By Pointer

```C++
//PASS BY VALUE
void inc(int *n)
{
    *n = *n + 1
}
...
int x = 5;
inc(&x);
cout << x; // Prints 6
```

#### Pass by Reference

`cin >> x` is the same as calling a function (let's call it `operator>>`) like so: `operator>>(cin, x)`

So in this case are we not passing the address of x?

C++ has a pointer-like type called a <u>reference</u>.

##### Lvalue Reference

An *lvalue* is something that can appear on the left hand side of an assignment. Something that has a memory address. For example `x` can be an *lvalue* since it has a memory address and has been initialized (where it was on the left side of the assinment `int x = 5`). Another example: `x+5` is not an *lvalue* since `x+5` does not have a memory address.

```c++
int y = 10;
int &z = y; // z is an lvalue reference to y
```

An lvalue reference is a constant pointer with automatic dereferencing.

`z` will continue to "point" to y forever. We can write `z=15` (notice no dereferencing) to change the value of `y` to 15.

If we write `*p = &z` then `p` now points to `y`.

Again, note that `z` has no identity of its own, it just mimics `y`.

so if we write`sizeof(z)`, this just gives the size of `y`.

###### Rules about references

- Cannot leave them uninitialized. (cannot write `int &z;`)
- Must initialize a reference with an *lvalue*
- Cannot create a pointer to a reference
- Cannot create a reference to ta reference.
- Cannot create an array of references.

Not allowed: `int &&x = ...;`, `int &*y = ...;`

Allowed: `int *&z = ...;`



We can now understand how pass by reference works:

```C++
void inc (int &n)
{
    n = n+1;
}
...
int x = 5;
inc(x);
cout << x; // Prints 6
```



So in the case of `cin`, the signature would look like:

```C++
std::istream &(std::istream &in, int &data);
```

Because we cannot make copies of streams we need parameter 2 (`in`) to be passed by reference. This way the vhanges to stream will be visible after the function is done.

We return the changes to `cin` by reference so that cascading can work: `cin >> x >> y` becomes `cin >> y` (here `cin` is the cin returned by the first call).



##### When to pass by reference

```C++
struct ReallyBig {...};

// Creates a copy of rb, which is resource expensive
void byVal(ReallyBig rb);

// In C we could pass by pointer to avoid the copy
// But in C++ we can just pass by reference
// Allows changes to the original
void byRef(ReallyBig &rb);

// What if we don't want to change the original though?
// This still avoids the copy
void byRefNoMutate(const ReallyBig &rb);
```

**Note:** always pass by reference to const when applicable

Keep in mind that we cannot call `byRef(5)` since 5 is not an *lvalue*.

HOWEVER, since C++ is a great language, it will allow you to call `byRefNoMutate(5)` since the signature indicates that we are not changing the *lvalue*.



### Dynamic Memory

Illegal in CS246:

```C++
int *p = malloc(some-size);
free(p)
```

Legal in CS246:

```C++

struct Node
{
    int data;
    Node *next;
}
...
Node *np = new Node {5, nullptr};
...
delete np;

int *arr = new int[10];
...
delete [] arr;
```



## Lecutre 8

```C++
Node getANode()
{
    Node n;
    return n; // a copy is returned (expensive)
}

Node *getANode()
{
    Node n;
    return &n; // this is garbage after the stackframe is popped (dangling pointer)
}
```

Never return a pointer or reference to data the function has allocated

Do this instead

```C++
Node *getANode()
{
    Node *np = new Node{...};
    return np;
}
```



### Operator Overloading

Idea: we can give meaning to C++ operators for user-defined types.

```C++
struct vec
{
    int x;
    int y;
};

...
// could do this:
vec v1 = add(a, b);
vec v2 = multiply(3, a);

// want to do this:
vec v1 = a + b;
vec v2 = 3 * a;

// so we implement with
vec operator+(const vec &a, const vec &b)
{
    vec v{a.x + b.x, a.y + b.y}
    return v;
}

vec operator*(int scalar, const vec &vector)
{
    return {scalar*vector.x, scalar*vector.y}
}

vec operator*(const vec &vector, int scalar)
{
    return scalar * vec;
}
```

```C++
struct Grade
{
    int theGrade;
};

//...
    
// if we wrote:
Grade g = {...};
cout << g << endl;
// it would not compile since cout doesnt know what to do with Grade

// we need to write:
ostream &operator<<(ostream &out, const Grade &g)
{
    out << g.theGrade << "%";
    return out;
}


// for input:
istream &operator>>(istream &in, Grade &g)
{
    in >> g.theGrade;
    if (g.theGrade < 0) g.theGrade = 0;
    if (g.theGrade > 100) g.theGrade = 100;
    return in;
}
```



### C/C++ Preprocessor

Program that runs and changes code before the compiler gets the code.

Anything beginning with a \# is a preprocessor directive

`#include <standard-library-package>` copy and pastes the file

`#include "file-in-current-directory"` copy and pastes the file



To run the preprocessor without compiling, run `g++14 -E -P filename.cc`



#### Other uses - #define

`#define VAR VALUE`

create a preprocessor variable with a value. Search every occurance of VAR and replace it with VALUE.



###### Obselete 'Search & Replace' Use

```C++
// PREVIOUS USE (NOW OBSELETE)
#define MAX 100
//...
int array[MAX];
```



###### Conditional Compilation Use

Suppose we want two levels of security. In level one, `key` has the type `shortInt`, in level two `key` has type `superLongInt`. We can write:

```C++
#define SECLEVEL 1 // either 1 or 2

int main()
{
    #if SECLEVEL == 1
    shortInt
    #elif SECLEVEL == 2
    superLongInt
    #endif
    publicKey;
}
```

When compiling we will get either:

```C++
int main()
{
    shortInt
    publicKey;
}
```

Or 

```C++
int main()
{
    superLongInt
    publicKey;
}
```

#### 

##### Set pre-processor variables from command line

This requires manually changing the `#define` for `SECLEVEL`. However, we can define pre processor variables from the command line.

```bash
g++ -DVAR=VALUE ...
```

From our example above, we would remove the `#define` and then write:

```bash
g++14 -E -P -DSECURITYLEVEL=2 preprocess.cc
```



#### Preprocessor Comment

```C++
#if 0
  // all code here is essentially commented out since it will never get compiled
#endif
```



#### Other Rules

```C++
#define VAR //not adding a value makes VAR represent empty string

#ifdef VAR //returns true if VAR is defined
#ifndef VAR //returns true if VAR is not defined

#define ever ;;
for(ever)
{
    
}
```



### Separate Compilation

Interface ( .h file )

- Type definitions and
- function declarations

Implementation ( .cc files )

- function implementations

**Look at files under `c++/separate/example1`**

How to compile when you have these files

1. `g++ *.cc`
2. `g++ main.cc vec.cc`

Headerfiles are NEVER compiled





## Lecture 9

When you have files that are dependent on eachother, and try to compile only one of them, you get a linker error.

By default g++ tries to compile, link, and then produce an executable.

To only compile,

`g++ -c main.cc`

`g++ -c vec.cc`

Produces object files (`.o`)

- Contains compiled code
- Contains information about what is defined and what is required

We can then do `g++ main.o vec.o` and the linker will simply check that all the "promises" have been kept.

Will need to keep track of which file(s) need to be recompiled.



### Makefile

- Specify the project dependencies
- Compile command

```makefile
#lectures/tools/make/vec-example1/Makefile

myprogram: main.o vec.o
	g++ main.o vec.o -o myprogram

vec.o: vec.cc vec.h
	g++ -std=c++14 -c vec.cc

main.o: main.cc vec.h
	g++ -std=c++14 -c main.cc

.PHONY: clean #this tells the program that "clean isn't a real file"

clean:
	rm *.o myprogram
```

```makefile
#lectures/tools/make/vec-example2/Makefile

CXX = g++
CXXFLAGS = -std=c++14 -Wall
OBJECTS = main.o vec.o
EXEC = myprogram

${EXEC}: main.o vec.o
	${CXX} ${CXXFLAGS} ${OBJECTS} -o ${EXEC}

vec.o: vec.cc vec.h

main.o: main.cc vec.h

.PHONY: clean

clean:
	rm ${OBJECTS} ${EXEC}

```

`g++ -MMD vec.cc` will create all the dependencies that the file needs by creating a `.d` file containing `vec.o : vec.cc vec.h` so we can simplify

```makefile
#lectures/tools/make/vec-example3/Makefile

CXX = g++
CXXFLAGS = -std=c++14 -Wall -MMD
EXEC = myprogram
OBJECTS = main.o vec.o
DEPENDS = ${OBJECTS:.o=.d}

${EXEC}: ${OBJECTS}
	${CXX} ${CXXFLAGS} ${OBJECTS} -o ${EXEC}

-include ${DEPENDS}

.PHONY: clean

clean:
	rm ${OBJECTS} ${EXEC} ${DEPENDS}
```



### Include Guards

Files: lectures/c++/separate/example3

```c++
//lectures/c++/separate/example3/vec.h
struct Vec {
  int x;
  int y;
};

Vec operator+(const Vec &v1, const Vec &v2);
```

`g++ -c linalg.cc` has multiple definitions of `Vec`.

How do we avoid multiple including things multiple times?

We use an include guard

```C++
//lectures/c++/separate/example4/vec.h

#ifndef VEC-H
#define VEC-H

struct Vec {
  int x;
  int y;
};

Vec operator+(const Vec &v1, const Vec &v2);

#endif
```



### Rules

Never compile `.h` files.

Never include `.cc` files

Do not put `using namespace std;` in `.h` files;



### C++ Classes

Big Idea for Object Oriented Programming in C++: putting functions inside `structs`

```c++
// student.h
#ifndef STUDENT-H
#define STUDENT-H
struct Student
{
    int assignmentsMark, midtermMark, finalMark;
    float grade(); // forward declaration
};
#endif
```

```c++
// student.cc

#include "student.h"

float Student::grade()
{
    return
        assignmentsMark * 0.4 +
        midtermMark * 0.2 +
        finalMark * 0.4;
}
```

`::` is the "scope resolution operator". So when we write `Student::` we are saying "in the scope of the Student structure..."

```C++
Student billy{80, 50, 70};
cout << billy.grade() << endl;
```

In object oriented programming, a class is a struct type that can contain functions. 

An instance of a class is called an **object** (e.g. billy is an object)

A function inside a class/struct is called a member function or a **method**.

A method can only be called using an object of the class.

Within a method, the method can access the fields of the object on which the method was called.

```c++
// student.cc

#include "student.h"

float Student::grade()
{
    return
        this->assignmentsMark * 0.4 +
        this->midtermMark * 0.2 +
        this->finalMark * 0.4;
}
```

A method contains a hidden parameter name, `this`. `this` points to the object on which the method was called.

`billy.grade()`

`this == &billy` and `*this == billy` 



### Initializing Objects

C style initialization: `Student billy{60, 50, 70}`

In C++ we can write special methods called **constructors** to construct objects.

```c++
// student.h
struct Student
{
    // notice no return type
    Student(int assignmentsMark, midtermMark, finalMark);
}
```

```c++
// student.cc
Student::Student(int assignmentsMark, midtermMark, finalMark)
{
    this->assignmentsMark = assignmentsMark;
    this->midtermMark = midtermMark;
    this->finalMark = finalMark;
}
```

# Lecture 10

```c++
// student.h
struct Student
{
    // notice no return type
    Student(int assignmentsMark, midtermMark, finalMark);
}
```

```c++
// student.cc
Student::Student(int assignmentsMark, midtermMark, finalMark)
{
    this->assignmentsMark = assignmentsMark;
    this->midtermMark = midtermMark;
    this->finalMark = finalMark;
}
```

```c++
Student billy{80,50,75};
//if an appropriate cunstructor is present, it is called

Student billy = Studnet(80,50,75)	//This does the same thing
```



### Uniform Initialization Syntax

```c++
int x = 5;				string s = "hello";
int x(5);				string s("hello");		//this does the same thing
```

```c++
//The desired syntax:
int x{5};
string s{"hello"};
Student billy{80,50,75};
```

```c++
//-------- on heap -------------
Student *pBilly = new Student{80,50,75};
delete pBilly;
```



### Advantages of writing cunstructors:

- Execute arbitary code
- default arguments
- overloading
- Sanity Checks

```c++
Struct Student{
    Student(int assns = 0, int mt = 0, int final = 0);
}
Student Student(int assns, int mt, int final){
    this->assns = assns < 0 ? 0 : assns;
    this->mt = mt < 0 ? 0 mt;
    this->final = final < 0 ? 0 final;
}

//Lets make some students
Student s1{50,60,70};
Student s2{50,60};
Student s3{50};
Student s4;		//no curly braces
```

### Default Constructors

Every class comes with a default (0 parameters) cunstructor, which initializes fields that are objects by calling its default constructor.

```c++
Struct A{
    int x;
    Student y;
    Vec *z;
}

A myA;
//calls the defualt cunstructor
//x,y are uninitialized
//y is initialized
```

As soon as you implement ANY cunstructor, you lose the default cunstructor and `C-style` initialization

```c++
struct vec{
    int x,y;
    Vec (int x, int y){
        this->x = x;		//implementation should be in .cc file 
        this->y = y;		//thus this is not the right style
    }
}

Vec v;	//won't compile
Vec v{0,0};	//this is good
```

### Initializing constant/reference fields

```c++
//---------- Option 1 ------------
int z; 
Struct Mystruct{
    const int myConst = 10;	//in class initialization
    int &myRef = z;			//in class initialization
}
```

```c++
//---------- Option 2 ------------
//Dp not do in class initialization
//Rule: const/refs must be initialized before constructor body runs
```

### Rules for object construction

1. Allocate space

2. field initialization (call default cunstructors for fields that are objects)

3. constructor body runs

   Lets hijack step 2: Member Initialization List (MIL)

```c++
Student::Student(const int, int asses, int mt, int final): 
	id{id}; assns{assns}; mt{mt}, final{final} {}
```

- MIL can be used for ALL fields
- no need to use 'this' to disambiguate fields/params with the same name
  - outside the braces the identifier is a field
  - inside the braces normal scope rules apply
- in the MIL field initialization occurs in field declaration order
- initializing fields in the MIL "can be" more efficient than initializing in the constructor body

### Initializing Objects as Copies of Others

```c++
Student billy{70,50,75};
Studnet bobby{billy};	//calls a copy constructor
```

### Things that come with every class:

1. default constructor
2. Copy constructor 
3. copy assignment operator
4. destructor
5. move constructor
6. move assignment operator

**2-6 are the Big 5, and a big part of the midterm is based on them

### The free copy constructor 

```c++
Student::Student(const Student &other):
	assns{other assns}, mt{other mt}, final{other final} {}
```

sometimes the free copy constructor does not work the way we want so we overwrite it

```c++
Struct Node{
    int data;
    Node *next;
    Node(int data, Node *next);
    Node(const Node &other):	//this is given for free so you don't need to write it
};
Node::Node(int data, Node *next): date{date}, next{next} {}
Node::Node(const Node &other): data{other data}, next{other next} {}
```

##### Now we will make a linked listp

```c++
Node *np = new Node{1, new Ndoe{2, new Node {3, nullptr}}};
Node m{*np};
//This is not a true copy of a linked list since it should share nodes
	//this is called a shallow copy
Node *n1 = new Node{*np};
//We want a deep copy
```

![](/Users/amirali/Downloads/image.jpg)

Often we want a deep copy

```c++
Node::Node(const Node &other):
data{other data},
next{new Node {*other next}} {}
next{other next ? new Node {*other next}: nullptr} {}
```

Copy constructor is called when:

1. an object is constructed as a copy of another
2. pass by value
3. return by value

Parameter of a copy constructor must be a reference

# Lecture 11

### The big 5 (continued)

Copy ctor: used to create copies of objects

Note on single parameter ctor

```c++
Node::Node(int data): data{data}, next{nullptr}{}


Node n{4};
Node n = 4;
```

One parameter ctors create implicit/automatic conversions

```c++
Voide foo(Node n){~~~~~}
foo(4);

string s="hello";
//this actually means
string s{"hello"};
```

To disable implicit conversions, use the "explicit" keyword:

```c++
Struct Node {
    explicit Node(int data);
};

Node n{4};	//would work
Node n = 4;	//would not work
foo(4);		//would not work
```



### Destructor(dtor)

A method called the destructor runs whenever objects are destroyed

​	for stack allocated:	when it goes out of scope

​	for heap allocated:	when delete is called on a ptr to the object

### Steps for object destruction

1. dtor body runs
2. fields that are objects are destroyed in reverse declaration order
3. dealocate space

the dtor body of the "free" dtor is empty, which might not be sufficient

`np -> [1| ] -> [2| ] -> [3|/]`
`delete np;`	

This calls dtor for Node1

- just dealocates node1
- leaks nodes 2 and 3

```c++
struct Node{
	~Node();	//no parametwers
			//cannot be overloaded
};
Node::~Node(){
    delete next;	//recursively deletes next node
}
```



### Copy assignment operator

```c++
Student billy{80,50,75};
Student bobby = billy;
Student jane;
jane = billy;		//copy assignment operator is used to update an existing object as a copy of another
//you could also do:
jane.operator = (billy);
```

```c++
Node &Node::operator=(const Node &other){
    if (this == &other) return *this;	//(2)
    data = other.date;
//X	next = new Node(*other.next);		//might cause dereferencing nullpointers
    delete next;						//(1)
    next = other.next ? new Node(*othr.next): nullptr;	//new Node(*othr.next):callToCopyCstr
    retrun *this;
}
```

1. Next could already be pointing to heap memory

   to avoid memory leaks, must deallocate objets

2. `Node n {1, new Node{2, nullptr}};`

   n = n;

   we will delete this->next and then try accessing it through other.next

   This causes a dangling ptr

   SO whenever operator deletes memory, use a self-assignment check



if the call to new Node() fails next is not assigned. But we deleted old next already. next is a dangling ptr.

```c++
Node &Node::operator(const Node &other){
    if(this==&other) returns *this;
    data = other.data;
    Node *tmp = next;		//first check for available memory before deleting anything
    next = other.next ? new Node(---): nullptr;
    delete tmp;
    return *this;
}
```



## Copy and Swap Idiom

```c++
struct Node {
    void swap (Node &other);
    
};

//-----node.cc-----
#include<utility>
voide Node::swap(Node &other){
    using std::swap;
    swap(data, other.data);
	swap(next, other.next);
}

Node &Node::operator = (const Node &other){
    Node tmp{other};	//deep copy
    this->swap(tmp);
    return *this;
}
```



```c++
//classes/rvalue/node.cc
Node plusOne(Node n){
    for (Node *p = &n; p; p=p->next) ++p ->data;
    return n;
}

Node n{1, new Node{2, nullptr}};	//2 calls to a 2 param ctor
Node n2{plusOne(n)};				//6 calls to copy ctor
```

