Create you own GCC cross compiler for your OS.
Download the necessary binutils and remember to set up the environment variables
to ensure the compiler build is able to detect our new binutils once we build them.

Assemble boot.s using:
i686-elf-as boot.s -o boot.o

Compile the written kernel using:

i686-elf-gcc -c kernel.c -o kernel.o -std=gnu99 -ffreestanding -O2 -Wall -Wextra

Linking the kernel(boot.o and kernel.o - the two object files generated)
i686-elf-gcc -T linker.ld -o myos.bin -ffreestanding -O2 -nostdlib boot.o kernel.o

verifying the grub file type:
grub-file --is-x86-multiboot myos.bin

Building a bootable cdrom image:
mkdir -p isodir/boot/grub
cp myos.bin isodir/boot/myos.bin
cp grub.cfg isodir/boot/grub/grub.cfg
grub-mkrescue -o myos.iso isodir

Load OS from a bootable CD_ROM using QEMU
qemu-system-x86_64 -cdrom /path/to/your/bootable-cd.iso
