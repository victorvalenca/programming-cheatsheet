# Programming Cheat Sheet 2016

This is my little cheat sheet for those moments when you're programming something and you just seem to blank out on what to do or how to do something.

I will build on this as I progress in my courses this semester, and possibly put them in separate files later on if it gets too hectic.

## Binary/Hex/Oct/Dec table (and 2 power table)
| BIN              | DEC            | OCT            | HEX            | POW            |
| :--------------- | :------------- | :------------- | :------------- | :------------- |
| `0000`           | 0              | 0              | 0              | 1              |
| `0001`           | 1              | 1              | 1              | 2              |
| `0010`           | 2              | 2              | 2              | 4              |
| `0011`           | 3              | 3              | 3              | 8              |
| `0100`           | 4              | 4              | 4              | 16             |
| `0101`           | 5              | 5              | 5              | 32             |
| `0110`           | 6              | 6              | 6              | 64             |
| `0111`           | 7              | 7              | 7              | 128            |
| `1000`           | 8              | 10             | 8              | 256            |
| `1001`           | 9              | 11             | 9              | 512            |
| `1010`           | 10             | 12             | A              | 1024           |
| `1011`           | 11             | 13             | B              | 2048           |
| `1100`           | 12             | 14             | C              | 4096           |
| `1101`           | 13             | 15             | D              | 8192           |
| `1110`           | 14             | 16             | E              | 16384          |
| `1111`           | 15             | 17             | F              | 32768          |
| `1 0000`         | 16             | 20             | 10             | 65536          |

***

## Initializing a fucking array
no seriously I trip up so much on this crap it's embarrassing

#### C:
```c
int arr[5]; // Declare
int arr[5] = {1,2,3,4,5}; // Declare & Initialize
int arr[5] = {0}; // Declare and initialize everything to 0 (or anything you like)
```

Remember to keep track of the array length, that is not tracked in the type level like in higher level languages. It's why you have argc/argv pairs among other things. Array length should (sadly) just be a parameter

#### C++:
```cpp
int arr[5]; // Declare
int arr[5] = {1,2,3,4,5}; // Declare & Initialize
```

#### Java:
```java
/* Declaring */
int[] arr; // int arr[] also works but is discouraged (from Oracle docs)

/* Initializing */
arr = new int[10];
```

***

## Complex C Declarations
| Declaration       | Meaning                                        | Example  |
| ----------------- | :--------------------------------------------- | :------- |
| `type name;`      | `name` is a `type`                             | `int a;` |
| `type *name;`     | `name` is a pointer                            | `int *a;`|
| `type **name;`    | `name` is a point to pointer to `type`         | `int **pp;`|
| `type name[];`    | `name` is an Array of `type`                   | `int a[3]; //a++; is illegal (a is a CONSTANT POINTER to the first element)`</p>`*(a+1) = 5; // This works, however` |
| `type *name[];`   | `name` is an Array of pointers                 | |
| `type (*name)[];` | Pointer to array of `type`                     | |
| `type name [][];` | `name` is array of array of `type`             | `*((a+i)+j) = 5 // Works`</p>`a[i][j] = 5; // Equivalent`|
| `type name();`    | Function `name` that returns `type`            | |
| `type *name();`   | Function `name` that returns pointer to `type` | |
| `type (*name)();` | Pointer to function `name` that returns `type`. A function pointer, yay!</p>A pointer to the first command of an array of CPU instructions (pretty much what a function is)</p>** Pointer arithmetic cannot be done on function pointers**                     | |
|~~`type(*name)()[];`~~</p>`type (*name[])();`| `name` is a pointer to an array of functions (cannot be done)</p>Name is an array of pointer to functions.| |
| `int *const p;`   | `p` is a constant pointer that returns `int`   | | |

### Getting practical  
```c
typedef int (*PTR_F)(int, int); // Pointer to function that takes two ints and returns type int
int (*ptr_f)(int, int);
PTR_F ptr_ft; // same as above
PTR_F aptr_ft[3];
/*
 *USAGE: ptf_f = ptf_ft = sum; where sum is name of function
 */
```

#### Array of pointer to function
```c
int (*aptr_f[3])(int, int);
```

#### Initialization or assignment
```c
ptr_f = ptr_ft = sum;
aptr_f[0] = aptr_ft[0] = sum;
aptr_f[1] = aptr_ft[1] = min;
aptr_f[2] = aptr_ft[2] = max;
```

#### "Exotic" declarations (I call these "mindfuck declarations")
```c
int (*(*foo(void))[]) (char);
int * (*(*(arr[3]) (void))[5];
```

#### Declaring a constant pointer
```c
int *const p;
```

#### EXAMPLE FROM table.h IN SCANNER ASSIGNMENT
```c
typedef Token (*PTR_AAF)(char *lexeme);
PTR_AAF aa_table[] = {NULL, NULL, aa_func02};
```

***

#### Malloc/calloc/free
```c
// needs std=gnu99
int *arr;
int len = 50;

arr = (int*)malloc(len * sizeof(int));
//     ^ the type you are containing
//                               ^ one pointer level down from the type you are containing
free(arr);

// calloc zeroes out any memory leftovers when allocating

arr = (int*)calloc(len, sizeof(int));

free(arr);

// ALWAYS FREE MEMORY YOU ALLOCATE. THE LANGUAGE WILL NOT DO THIS FOR YOU
// (Most profs hate it and will cut you, and your grade for it)
```

***

## Makefiles and GCC

### Makefiles

#### tl;dr:

```makefile
output-file: input-file other-input-file
    lol compiler commands here
```

#### Constants:
An example:
```makefile
CC=gcc
CFLAGS=-I. -O3
```

To use constants in a rule, use `$(CONSTANT)`. Much like a `#define` in C/C++, it will be replaced verbatim

To have a literal dollar sign ($), do `$$`.

#### Practical example

```makefile
CC=gcc
CFLAGS=-I. -O3

smooth-criminal: michael_jackson.c lyrics.c
    $(CC) -O smooth-criminal michael_jackson.c lyrics.c $(CFLAGS)
```

A good suggestion for beginners (like me) is to map your project on paper so you can visualize your source files and dependencies properly (very useful for more complex projects). It will most certainly prevent you from bashing your head on a wall in anger trying to figure out how to write your makefile rules.

Note: Running `$ make` without arguments will run the first rule in the makefile

### GCC

#### Options most commonly used
| Option      | "What does this button do?" | What do you get out of it? |
| :---------- | :-------------------------- | :------------------------- |
| `-c`        | Only runs preprocess, compile, and assembly steps. | You get an assembly file for the given architecture used by the compiler (it varies by compiler, but for GCC it uses x86 or x86_64, depending on your OS) |
| `-g`        | Generates source-level debug information | You can debug your shit now |
| `-o <file>` | Names the file the executable will be written to | Your executable is not named `a.out` |
| `-j<value>`   | Number of concurrent jobs to be done. Default is 1, and it is unlimited | You can do more shit at the same time. Usually not a big deal with smaller projects, but you can see some build speed increase on large projects. Recommended to never give a value higher than your logical core count (CPU thread count, not physical cores) <p> **Set this too high a number and mechanical hard drives will beg for mercy when used with large projects** |

#### Flags you will/should care about

| Flag                       | Why should I give a damn about it? |
|:-------------------------- |:---------------------------------- |
| `-Werror`                  | Treats all compiler warnings as errors. Prevents a lot of stupid mistakes from even compiling. |
| `-Wall`                    | Enables literally every single warning the compiler can throw at you. Combined with `-Werror`, it is literal hardmode programming |
| `-Wmisleading-indentation` | Has the compiler tell you if you're abusing C's block rules to make code that looks okay to a human. However it can cause things like the `goto fail` vulnerability. |
| `-I <value>`               | Adds a header search location that is searched _before_ the system header lists. This is for angle bracket includes. |
| `-std=gnu11`               | Sets the C standard to use when compiling to gnu99. Your professor may mandate otherwise. Generally you should be using gnu99 or gnu11 (gnu extensions to c99 and c2011). |

***

## Git gud
Another bane of my existence. I'm writing this so I don't have to hunt down the guide on GH every time I forget. Writing it out also helps me remember!

### Creating a local repository (with terminal)

1. `$ cd` into root of project
2. `$ git init`. Git will create a `.git` directory if it works
3. `$ git add` to prepare ~~these humans for ascension~~ files for commit
4. `$ git commit`

Good job mate, you have a local git repo.

> But how the heck do I put it on GitHub?

WELLLLLLL don't get ahead of yourself, pal.

1. Go to github, make a new repository (depending on how you set your local repo up, you may or may not necessarily need a README.md, so uncheck that little bugger if so)
2. Back to your local repo:
  1. `$ git remote add origin git@github.com:username/repo_name`
    - Note: This is the SSH method. If you have that set up already you won't have to put in your username/password every time you want to push things. The normal method is:
    `$ git remote add origin https://github.com/username/repo_name`
  2. `$ git push -u origin master`

*Remember the three magic words: **add, commit, push**.*
