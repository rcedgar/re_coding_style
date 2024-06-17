# re_coding_style

# Robert C. Edgar's coding style, mostly C++

### Local, member and global variable names

Member variables have the `m_` prefix, globals are named `g_`, local variables have no special prefix.
Static global variables are not distinguished from exported variables (probably better would be to
use `s_` for static global).

### Error handling

Exceptions must not be used (see below). Flow-of-control statements for handling errors must be minimized; a given
function should have have either (a) error control flow or (b) algorithm flow, but not both. This is typically
implemented by having small functions which quit with a fatal error if an operation fails, for example
OpenStdioFile() will call Die() if it fails so that the calling code does not have to check for a zero return
code, it can continue assuming the file was opened successfully.

### Exceptions never used

Reading Scott Meyers' Effective C++ books convinced me that writing robust, multi-threaded C++ code with exception handling
was essentially impossible. Certainly way beyond the amount of effort I was willing to invest in figuring out reliable
design patterns and coding habits.

### Hard-coded lists should behave like data not code

Lists of things crop up often in practical C++ code. A good example is command-line options (see below). If you can write
an enum with the list once, and this covers all the use-cases, fine. But it often doesn't, with the result that similar
lists enumarating all the list members crop up repeatedly, e.g. switch statements with one case per member having a
cookie-cutter statement. Maintaining scattered uses of essentially the same list is tedious and error prone. 
Therefore, I often use an #include hack so that the list is found only one place.

### Command-line options

I strongly dislike the typical command-line parsing in most C and C++ programs I see in bioinformatics. They are hard-to-read
and hard-to-maintain spaghetti.

### Memory allocation

### Assertions

### Template classes

### Modern C++

### Terse constructs avoided

### Integers are usually `uint`

blah

### The `SIZE()` macro

### Looping over containers

### Portability to Linux, Windows and OSX

### `myutils.cpp` and `myutils.h`

### CamelCase identifiers

I generally use CamelCase. Recently I've found that I prefer typical python 
style (see e.g. [https://google.github.io/styleguide/pyguide.html](https://google.github.io/styleguide/pyguide.html)
because CamelCase requires way too many Shift-key presses making it harder to type.
