# Bare-metal-teensy-3.x
This allow you to program to your teensy 3.x without the use of arduino studio. You can both use c and c++ files. 

The folder teensy3 contains a part for the Teensyduino, it is the bare form there core library. But there are som små changes. The linker files are moved to the folder project and the mk20dx128.c file is also moved to the folder project and renamed to startup.c because it contains the interrupt vector and reset function for all teensy 3. 

The folder tools contains the teensy loader from [www.pjrc.com/teensy]. On linux is it a good idea to copy the 49-teensy.rules to /etc/udev/rules.d/


In the folder project you find the make file, which compiles your program. You should change the variable LIBDIR to your library path (The folder teensy3 or others) and TARGET. 

The make file has som function you can call. 

`make` 
Compile the project to a hex file

`make install` 
Install the compiler gcc-arm-nano-eabi (works only on linux systems)

`make uninstall`
Uninstall the compiler (works only on linux systems)

`make setup` 
Create a src and build folder (build folder not used right now)
`make fuse`
flash the hex files to the teensy with teensy loader

`make clean`
Remove the files with the externsions *.d *.o *.elf og *.hex
