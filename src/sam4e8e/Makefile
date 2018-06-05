# Additional sam3x8e build rules

# Setup the toolchain
CROSS_PREFIX=arm-none-eabi-

dirs-y += src/sam4e8e src/generic
dirs-y += lib/cmsis-sam4e8e/source lib/cmsis-sam4e8e/source/gcc

CFLAGS += -mthumb -mcpu=cortex-m4
CFLAGS += -Ilib/cmsis-sam4e8e/include -Ilib/cmsis-sam4e8e/cmsis-include
CFLAGS += -D__SAM4E8E__

CFLAGS_klipper.elf += -Llib/cmsis-sam4e8e/source/gcc
CFLAGS_klipper.elf += -T lib/cmsis-sam4e8e/source/gcc/flash.ld
CFLAGS_klipper.elf += --specs=nano.specs --specs=nosys.specs

# Add source files
src-y += sam4e8e/main.c sam4e8e/timer.c sam4e8e/gpio.c sam4e8e/spi.c
src-y += generic/crc16_ccitt.c generic/alloc.c
src-y += generic/armcm_irq.c generic/timer_irq.c
src-y += ../lib/cmsis-sam4e8e/source/system_sam4e.c
src-y += ../lib/cmsis-sam4e8e/source/gcc/startup_sam4e.c
src-$(CONFIG_SERIAL) += sam4e8e/serial.c generic/serial_irq.c

# Build the additional hex output file
target-y += $(OUT)klipper.bin

$(OUT)klipper.bin: $(OUT)klipper.elf
	@echo "  Creating bin file $@"
	$(Q)$(OBJCOPY) -O binary $< $@

flash: $(OUT)klipper.bin
	@echo "  Flashing $^ to $(FLASH_DEVICE) via bossac"
	$(Q)stty -F "$(FLASH_DEVICE)" 1200
	$(Q)bossac -i -p "$(FLASH_DEVICE:/dev/%=%)" -R -e -w -v -b $(OUT)klipper.bin
