# put your *.o targets here, make should handle the rest!
SRCS = main.c system_stm32f4xx.c stm32f4xx_it.c 

# all the files will be generated with this name (main.elf, main.bin, main.hex, etc)
PROJ_NAME=main

# Location of the Libraries folder from the STM32F0xx Standard Peripheral Library
STD_PERIPH_LIB		=	../../STM32F4xx__StandardDrivers
USER_PERIPH_LIB 	= 	../../STM32F4xx_userDrivers

# Location of the linker scripts
LDSCRIPT_INC= ../Device-specific/ldscripts

# output folder name
BIN = bin

# object files folder name
OBJ_FOLDER = obj

# that's it, no need to change anything below this line!

###################################################

CC		=	arm-none-eabi-gcc
OBJCOPY	=	arm-none-eabi-objcopy
OBJDUMP	=	arm-none-eabi-objdump
SIZE	=	arm-none-eabi-size

#CFLAGS  = -Wall -g -std=c99 -Os  
#CFLAGS += -mlittle-endian -mthumb -mcpu=cortex-m0 -march=armv6s-m
#-mfpu=fpv4-sp-d16 -mfloat-abi=hard
#CFLAGS += -mcpu=cortex-m4 -mthumb -mlittle-endian  -mthumb-interwork
#CFLAGS += -DUSE_STDPERIPH_DRIVER -DSTM32F4XX -DSTM32F401xE


CFLAGS  = -ggdb -O0 -Wall -Wextra -Warray-bounds
CFLAGS += -mcpu=cortex-m4 -mthumb -mlittle-endian -mthumb-interwork -std=gnu11
CFLAGS += -mfloat-abi=hard -mfpu=fpv4-sp-d16
CFLAGS += -DUSE_STDPERIPH_DRIVER -DSTM32F4XX -DSTM32F401xC
CFLAGS += -T$(LDSCRIPT_INC)/stm32_flash.ld
#CFLAGS += -ffunction-sections -fdata-sections
CFLAGS += -Wl,--gc-sections -Wl,-Map=../$(BIN)/$(PROJ_NAME).map

###################################################

vpath %.c src
vpath %.a $(STD_PERIPH_LIB)

ROOT=$(shell pwd)

CFLAGS += -I inc -I $(STD_PERIPH_LIB) -I $(STD_PERIPH_LIB)/CMSIS/Device/ST/STM32F4xx/Include
CFLAGS += -I $(USER_PERIPH_LIB)/include
CFLAGS += -I $(STD_PERIPH_LIB)/CMSIS/Include -I $(STD_PERIPH_LIB)/STM32F4xx_HAL_Driver/Inc
CFLAGS += -include $(STD_PERIPH_LIB)/stm32f4xx_hal_conf.h -I ../includes

SRCS += ../Device-specific/startup_stm32f401xc.s # add startup file to build

SRCS += $(foreach dir, $(USER_PERIPH_LIB)/src, $(wildcard $(dir)/*.c))

# need if you want to build with -DUSE_CMSIS 
#SRCS += stm32f0_discovery.c
#SRCS += stm32f0_discovery.c stm32f0xx_it.c

OBJS = $(SRCS:.c=.o)

.c.o:
	@ $(CC) $(CFLAGS) -c $< -o ../$(OBJ_FOLDER)/
	@ echo "compiling src files...."

###################################################

.PHONY: lib proj

all: build proj

build:
	@ echo "created obj and bin directories for this project"
	@ mkdir -p ../$(BIN) ../$(OBJ_FOLDER)

lib:
	@ echo "compiling standard library files..."
	@ $(MAKE) -C $(STD_PERIPH_LIB)

proj: 	$(PROJ_NAME).elf

$(PROJ_NAME).elf: $(SRCS) | lib
	@ echo "successful compilation of src files"
	@echo "linking object files"
	@ echo "creating elf file..."
	@ $(CC) $(CFLAGS) $^ -o ../$(BIN)/$(PROJ_NAME).elf
	@ echo "successful linking of object files"
	@ echo "created elf file successfully"
	@ echo "creating hex file..."	
	@ $(OBJCOPY) -O ihex ../$(BIN)/$(PROJ_NAME).elf ../$(BIN)/$(PROJ_NAME).hex
	@ echo "created hex file successfully"
	@ echo "creating bin file..."
	@ $(OBJCOPY) -O binary ../$(BIN)/$(PROJ_NAME).elf ../$(BIN)/$(PROJ_NAME).bin
	@ echo "created bin file successfully"
	@ echo "Memory size consumed with this application is: "
	@ $(SIZE) ../$(BIN)/$(PROJ_NAME).elf
	
clean:
	@ find ./ -name '*~' | xargs rm -rf	
	@ rm -rf ../$(OBJ_FOLDER)/*.o
	@ echo "successfully removed project object files"
	@ rm -rf ../$(BIN)/$(PROJ_NAME).elf
	@ echo "successfully removed elf file"
	@ rm -rf ../$(BIN)/$(PROJ_NAME).hex
	@ echo "successfully removed hex file"
	@ rm -rf ../$(BIN)/$(PROJ_NAME).bin
	@ echo "successfully removed bin file"
	@ rm -rf ../$(BIN)/$(PROJ_NAME).map
	@ echo "successfully removed map file"
	@ rm -rf ../$(BIN)/$(PROJ_NAME).lst
	@ echo "successfully removed lst file"
