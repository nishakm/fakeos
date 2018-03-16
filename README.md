# FakeOS: Implementation and notes based on educational courses available online

## About the Author's Abilities
This is my first real foray into Operating Systems. It has been years since I programmed in DOS. I have been develping on a Linux based machine for years now and hence my current experience is the reference point I choose to learn something new. With that in mind, here is my reference should anyone else want to use it:

- Fedora VM
- Ubuntu native install

## Learning Material
FakeOS is based on the MIT online course on operating systems
This does not follow any of the lab implementations but it does use some material from the first lab

## Getting Started on Fedora
- Install git and gcc (sudo dnf install git gcc)
- Install qemu (sudo dnf install qemu)

The qemu install has a list of binaries that go into /usr/bin/ and are prefixed by qemu-. The one I used is qemu-system-x86_64. The native architecture I use is x86_64. I haven't gotten to cross compiling at this point in time

Later on in the lab I changed it to qemu-system-i386 as I found out that the kernel was compiled using CFLAGS as -m32 (meaning compiled for a 32bit processor) and gdb was erroring out because the memory address sent to it was too long. I do not know what CFLAGS means.

Lab1 of the MIT course requires you to untar lab1 which is a tar of the git history. I set the QEMU environment variable to the above qemu, and then ran make qemu, getting the correct results. See lab1 from the MIT study guide.

```
$ make qemu       
/usr/bin/qemu-system-x86_64 -hda obj/kern/kernel.img -serial mon:stdio -gdb tcp::26000 -D qemu.log 
WARNING: Image format was not specified for 'obj/kern/kernel.img' and probing guessed raw.
         Automatically detecting the format is dangerous for raw images, write operations on block 0 will be restricted.
         Specify the 'raw' format explicitly to remove the restrictions.      

(qemu-system-x86_64:16448): Gtk-WARNING **: Allocating size to GtkScrollbar 0x564667812340 without calling gtk_widget_get_preferred_width/height(). How does the code know the size to allocate?
6828 decimal is XXX octal!             
entering test_backtrace 5              
entering test_backtrace 4              
entering test_backtrace 3              
entering test_backtrace 2              
entering test_backtrace 1              
entering test_backtrace 0              
leaving test_backtrace 0               
leaving test_backtrace 1               
leaving test_backtrace 2               
leaving test_backtrace 3               
leaving test_backtrace 4               
leaving test_backtrace 5               
Welcome to the JOS kernel monitor!     
Type 'help' for a list of commands.    
K> help            
help - Display this list of commands   
kerninfo - Display information about the kernel                               
K> kerninfo        
Special kernel symbols:                
  _start                  0010000c (phys)                                     
  entry  f010000c (virt)  0010000c (phys)                                     
  etext  f01019a9 (virt)  001019a9 (phys)                                     
  edata  f0112300 (virt)  00112300 (phys)                                     
  end    f0112944 (virt)  00112944 (phys)                                     
Kernel executable memory footprint: 75KB
```
If you run make qemu-nox you won't get the annoying VGA window and the Gtk-WARNING will not come up. But you will not be able to CTRL+C out of this as QEMU probably owns the terminal session. Lab1 says that the kernel writes to both the QEMU's VGA port as well as its serial port which in turn uses the terminal standard out.

When running gdb I get a warning about the auto-load-safe-path of the .gdbinit in the lab directory was declined by some other auto-load-safe-path setting. The tool said to create a ~/.gdbinit and set the auto-load-safe-path value to the lab's .gdbinit file

```
$ vim ~/.gdbinit
add-auto-load-safe-path /home/username/path/to/lab/.gdbinit
```

## A Note About Memory

Memory locations are addressable here using 32 bits. In x86_64 CPUs they are addressed using 64 bits. For a 32 bit processor the memory can go up to 2^32 address locations. meaning 4294967296 Bytes (if each memory location can hold 8 bits). Divide the number by 1024 (rather than 1000) 3 times and you get 4 GiB of memory. All the memory ranges described in lab1 follow hexadecimal memory mapping and factoring by 1024 rather than 1000 to calculate KiB and MiB.

Key things to remember:

- 0x00000000 - 0x000A0000 (640KiB) Low Memory/Conventional Memory
- 0x000A0000 - 0x00100000 (384KiB) Memory hole
- 0x00100000 -> (RAM limit) Extended Memory
- 0xFFFFFFFF -> (whatever BIOS reserves) For memory mapped 32 bit PCI devices
- 0x000F0000 - 0x00100000 (64KiB) Where BIOS used to be (they life in flash memory now)

## Sudy Guides
- [MIT Online Course](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-828-operating-system-engineering-fall-2012/)

## References
- [Brennan's Guide to Inline Assemby Intel vs AT&T Syntax](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html)
- [Intel 80386 Programmer's Reference Manual](http://www.logix.cz/michal/doc/i386/)
- [Comprehensive Intel 64 and IA-32 Reference Manuals](https://software.intel.com/en-us/articles/intel-sdm)
- [AMD64 Architecture Reference Manual](https://refspecs.linuxfoundation.org/LSB_3.1.0/LSB-Core-AMD64/LSB-Core-AMD64/normativerefs.html)
