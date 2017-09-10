#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
 
BASENAME = c1m2
TARGET = $(BASENAME).out

# Architectures Specific Flags
LINKER_FILE = ../msp432p401r.lds

# Compiler Flags and Defines


	ifeq ($(PLATFORM),HOST)
		CC = gcc
		LD = ld
		LDFLAGS = -Wl,-Map=$(BASENAME).map
		CPPFLAGs = -DHOST
		CFLAGS = -Wall -Werror -g -O0 -std=c99	
		
	else ifeq ($(PLATFORM),MSP432)

		CPU = cortex-m4
		ARCH = thumb
		ARCH2 = armv7e-m
		FLOAT = hard
		FPU = fpv4-sp-d16
		SPECS = nosys.specs 
		CC = arm-none-eabi-gcc
		LD = arm-none-eabi-ld
		LDFLAGS = -Wl,-Map=$(BASENAME).map -T $(LINKER_FILE) 
		CPPFLAGs = -DMSP432
		CFLAGS = -mcpu=$(CPU) -m$(ARCH) --specs=$(SPECS)\
			 -march=$(ARCH2) -mfloat-abi=$(FLOAT)\
			 -mfpu=$(FPU) -Wall -Werror\
				-g\
				-O0\
				-std=c99
	endif				
		

 

OBJS = $(SOURCES:.c=.o)

%.i : %.c
	$(CC)  $(CPPFLAGs)  $(INCLUDES) -E  -o $@  $<
%.asm : %.c
	$(CC)  $(CPPFLAGs)  $(INCLUDES) -S  -o $@  $<
%.o : %.c
	$(CC)  $(CPPFLAGs) -c $<  $(INCLUDES) $(CFLAGS) 

.PHONY: compile-all
compile-all: $(TARGET)
	$(CC)  $(CPPFLAGs) -c $(OBJS)  $(INCLUDES) $(CFLAGS) 

.PHONY: build
build : $(TARGET)

$(TARGET) : $(OBJS)
	$(CC)  $(CPPFLAGs)  $(OBJS)  $(INCLUDES) $(CFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(TARGET) $(BASENAME).map *.o *.asm *.i
