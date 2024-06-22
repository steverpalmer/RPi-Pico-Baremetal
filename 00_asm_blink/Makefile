# Copyright (c) 2024 CarlosFTM
# This code is licensed under MIT license (see LICENSE.txt for details)
NAME    = asm-blink
CPU     = cortex-m0plus
ARMGNU  = arm-none-eabi
AFLAGS  = --warn --fatal-warnings -mcpu=$(CPU) -g
LDFLAGS = -nostdlib
PICO_SDK_PATH ?= ~/pico/pico-sdk
ELF2UF2 ?= $(PICO_SDK_PATH)//tools/elf2uf2/elf2uf2

all: $(NAME).bin

$(NAME).o: $(NAME).s
	$(ARMGNU)-as $(AFLAGS) $(NAME).s -o $(NAME).o

$(NAME).bin: boot_stage2.ld $(NAME).s $(NAME).o
	$(ARMGNU)-ld $(LDFLAGS) --entry 0x20040001 -T boot_stage2.ld $(NAME).o -o $(NAME).elf
	$(ARMGNU)-objdump -D $(NAME).elf > $(NAME).list
	$(ARMGNU)-objcopy -O binary $(NAME).elf $(NAME).bin
	$(ELF2UF2) $(NAME).elf $(NAME).uf2

clean:
	rm -f *.bin *.o *.elf *.list *.uf2
