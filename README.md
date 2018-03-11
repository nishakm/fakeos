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

Lab1 of the MIT course requires you to untar lab1 which is a tar of the git history. I set the QEMU environment variable to the above qemu, and then ran make qemu getting the correct results.

## Sudy Guides
- [MIT Online Course](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-828-operating-system-engineering-fall-2012/)

## References
- [Brennan's Guide to Inline Assemby Intel vs AT&T Syntax](http://www.delorie.com/djgpp/doc/brennan/brennan_att_inline_djgpp.html)
- [Intel 80386 Programmer's Reference Manual](http://www.logix.cz/michal/doc/i386/)
- [Comprehensive Intel 64 and IA-32 Reference Manuals](https://software.intel.com/en-us/articles/intel-sdm)
- [AMD64 Architecture Reference Manual](https://refspecs.linuxfoundation.org/LSB_3.1.0/LSB-Core-AMD64/LSB-Core-AMD64/normativerefs.html)
