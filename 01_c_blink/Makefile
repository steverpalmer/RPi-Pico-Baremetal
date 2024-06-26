# Copyright (c) 2024 CarlosFTM
# This code is licensed under MIT license (see LICENSE.txt for details)
NAME    = c-blink
CPU     = cortex-m0plus
ARMGNU  = arm-none-eabi
AFLAGS  = --warn --fatal-warnings -mcpu=$(CPU) -g
LDFLAGS = -nostdlib
CFLAGS  = -mcpu=$(CPU) -nostartfiles -ffreestanding -g -O0 -fpic -mthumb -c
PICO_SDK_PATH ?= ~/pico/pico-sdk
ELF2UF2 ?= $(PICO_SDK_PATH)/tools/elf2uf2/elf2uf2

all: $(NAME).bin

$(NAME)-asm.o: $(NAME)-asm.s
	$(ARMGNU)-as $(AFLAGS) $(NAME)-asm.s -o $(NAME)-asm.o

$(NAME)-main.o: $(NAME)-main.c
	$(ARMGNU)-gcc $(CFLAGS) $(NAME)-main.c -o $(NAME)-main.o

$(NAME).bin: boot_stage2.ld $(NAME)-asm.o $(NAME)-main.o
	$(ARMGNU)-ld $(LDFLAGS) --entry 0x20040001 -T boot_stage2.ld $(NAME)-asm.o $(NAME)-main.o -o $(NAME).elf
	$(ARMGNU)-objdump -D $(NAME).elf > $(NAME).list
	$(ARMGNU)-objcopy -O binary $(NAME).elf $(NAME).bin
	$(ELF2UF2) $(NAME).elf $(NAME).uf2

clean:
	rm -f *.bin *.o *.elf *.list *.uf2
