# re_coding_style

# Robert C. Edgar's C++ coding style

### Local, member and global variable names

Member variables have the `m_` prefix, globals are named `g_`, local variables have no special prefix.
Static global variables are not distinguished from exported variables (probably better would be to
use `s_` for static global). Command-line options are `opt_name` or `opt(name)`.

### Error handling

Exceptions must not be used (see below). Flow-of-control statements for handling errors must be minimized; a given
function should have have either (a) error control flow or (b) algorithm flow, but not both. This is typically
implemented by having small functions which quit with a fatal error if an operation fails, for example
`OpenStdioFile()` will call `Die()` if it fails so that the calling code does not have to check for a zero return
code, it can continue assuming the file was opened successfully.

### Exceptions never used

Reading Scott Meyers' Effective C++ books convinced me that writing robust, multi-threaded C++ code with exception handling
was close to impossible. Certainly impractical for me; way beyond the amount of effort I was willing to invest in
figuring out reliable design patterns and coding habits.

### Hard-coded lists should behave like data not code

Lists of things crop up often in my kind of C++ code. A good example is command-line options (see below). If you can write
an enum with the list once, and this covers all the use-cases, fine. But often it doesn't, with the result that coding
effectively all its members crop up repeatedly, e.g. switch statements with one case per member having a
cookie-cutter statement. Maintaining scattered uses of essentially the same list is tedious and error prone. 
Therefore, I often use an #include hack so that the list is maintained only one place. Maybe modern C++ has
better solutions; please let me know if you have a suggestion (open an issue in this repo).

### Command-line options

I strongly dislike the typical command-line parsing in most C and C++ programs I see in bioinformatics. They are hard-to-read
and hard-to-maintain spaghetti. I prefer *al dente*. I want the command-line parsing as such to be written once and forgotten,
cleanly separated from the list of options. Using an option should have minimal run-time overhead such that they can be accessed
anywhere except perhaps innermost loops. Warnings should be issued for unused options to mitigate the difficulty in
documenting which options apply to which commands. The set of options is a list requiring #include hacks (see above).
A variable which is an option should be evident in the code; this is achieved by calling a global variable `opt_name` or
by using macro `opt()` (only one of these styles applies, depending on which version of `myutils` is included).
A global boolean `optset_name` is used to check if the option was set on the command line.

### Commands

A command is a special case of a command-line option. It always takes a string value. The list 
of commands is in `cmd.h`.
The main program will call a function named `cmd_name()` for command `name`, e.g. if the command line
starts with `-usearch_global` then `main()` will call `cmd_usearch_global()` with no arguments. To access
the value, use `opt(usearch_global)` or `opt_usearch_global` depending on the version of `myutils`.
In more recent code, the global variable `g_Arg1` has the string value of the command option.

### Memory allocation

I use `myalloc` and `myfree` to wrap `malloc` and `free` so that fatal errors can be issued; calling code can
assume that the functions succeeded. `myalloc` is a macro, usage is `myalloc(typename, n)` to allocate
a bufferfor a vector of length `n`, e.g. `myalloc(int, 10)`.

### Assertions

I assert everything I can think of, usually with `asserta` ("assert always", i.e. even in a Release build)
unless there is likely to be a meaningful execution penalty, i.e. the assertion is in a
critical inner loop.

### Separating interface from implementation

I prefer to separate interface from implementation. For example, template classes are very powerful, but
I try to avoid them because the interface is hard to see and the implementation must be written as a 
long header file which I find difficult to navigate and to think about when I'm looking at it.
I do use them in a few cases, e.g. `Mx<>` in `mx.h`. For non-trivial code,
I prefer short functions in short source files; then the code I'm looking at does one well-defined
job without being surrounded by distracting noise. Sometimes my `.cpp` files do get quite long, 
in a weak violation of this guideline, but in these cases I usually have a shortish `.h` file defining
a class interface, and Visual Studio allows me to jump straight to the member function by pressing `[F12]`
so I don't have to scroll around to find it.

### Modern C++

My C++ syntax mostly fossilized in the early 2000's. This is partly laziness (I haven't bothered
to learn the new stuff) and partly for practical reasons: using a conservative subset of C++
improves portability and helps make it accessible for collaborators having varying expertise.

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

### Dependencies

