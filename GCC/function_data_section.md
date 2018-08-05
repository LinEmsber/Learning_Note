# Compilation options

## Description
  * The operation of eliminating the unused code and data from the final executable is directly performed by the linker.

  * In order to do this, it has to work with objects compiled with the following options: -ffunction-sections -fdata-sections.

  * These options are usable with C and Ada files. They will place respectively each function or data in a separate section in the resulting object file.

  * Once the objects and static libraries are created with these options, the linker can perform the dead code elimination. You can do this by setting the -Wl,--gc-sections option to gcc command or in the -largs section of gnatmake. This will perform a garbage collection of code and data never referenced.

## Reference
  *[Compilation-options](https://gcc.gnu.org/onlinedocs/gnat_ugn/Compilation-options.htm)
