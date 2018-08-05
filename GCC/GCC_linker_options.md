# Options for Linking
## Options

### -fuse-ld=bfd
  * Use the bfd linker instead of the default linker.

### -llibrary
  * Search the library named library when linking. (The second alternative with the library as a separate argument is only for POSIX compliance and is not recommended.)

  * It makes a difference where in the command you write this option; the linker searches and processes libraries and object files in the order they are specified. Thus, ‘foo.o -lz bar.o’ searches library ‘z’ after file foo.o but before bar.o. If bar.o refers to functions in ‘z’, those functions may not be loaded.

  * The linker searches a standard list of directories for the library, which is actually a file named liblibrary.a. The linker then uses this file as if it had been specified precisely by name.

### -nostartfiles
  * Do not use the standard system startup files when linking. The standard system libraries are used normally, unless -nostdlib, -nolibc, or -nodefaultlibs is used.

### -nolibc
  * Do not use the C library or system libraries tightly coupled with it when linking. Still link with the startup files, libgcc or toolchain provided language support libraries such as libgnat, libgfortran or libstdc++ unless options preventing their inclusion are used as well. This typically removes -lc from the link command line, as well as system libraries that normally go with it and become meaningless when absence of a C library is assumed, for example -lpthread or -lm in some configurations. This is intended for bare-board targets when there is indeed no C library available.

### -nostdlib
  * Do not use the standard system startup files or libraries when linking. No startup files and only the libraries you specify are passed to the linker, and options specifying linkage of the system libraries, such as -static-libgcc or -shared-libgcc, are ignored.

### -pthread
  * Link with the POSIX threads library. This option is supported on GNU/Linux targets, most other Unix derivatives, and also on x86 Cygwin and MinGW targets. On some targets this option also sets flags for the preprocessor, so it should be used consistently for both compilation and linking.

### -static
  * On systems that support dynamic linking, this overrides -pie and prevents linking with the shared libraries. On other systems, this option has no effect.

### -shared
  * Produce a shared object which can then be linked with other objects to form an executable. Not all systems support this option. For predictable results, you must also specify the same set of options used for compilation (-fpic, -fPIC, or model suboptions) when you specify this linker option.
### -Wl,option
  * Pass option as an option to the linker. If option contains commas, it is split into multiple options at the commas. You can use this syntax to pass an argument to the option. For example, -Wl,-Map,output.map passes -Map output.map to the linker. When using the GNU linker, you can also get the same effect with -Wl,-Map=output.map.

## Reference
  * [GCC Link-Options](https://gcc.gnu.org/onlinedocs/gcc/Link-Options.html)
