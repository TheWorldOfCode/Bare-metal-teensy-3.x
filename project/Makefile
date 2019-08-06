# Detect OS 
OS2 := $(shell uname)

# set your MCU type here, or make command line `make MCU=MK20DX256`
MCU=MK20DX256
#MCU=MKL26Z64
#MCU=MK64FX512
#MCU=MK66FX1M0

# make it lower case
LOWER_MCU := $(subst A,a,$(subst B,b,$(subst C,c,$(subst D,d,$(subst E,e,$(subst F,f,$(subst G,g,$(subst H,h,$(subst I,i,$(subst J,j,$(subst K,k,$(subst L,l,$(subst M,m,$(subst N,n,$(subst O,o,$(subst P,p,$(subst Q,q,$(subst R,r,$(subst S,s,$(subst T,t,$(subst U,u,$(subst V,v,$(subst W,w,$(subst X,x,$(subst Y,y,$(subst Z,z,$(MCU)))))))))))))))))))))))))))

MCU_LD = $(LOWER_MCU).ld

# The name of your project (used to name the compiled .hex file)
TARGET = main
# Path to libs 
LIBDIR = $(abspath ../../cores-master/teensy3) 
# Path to teensy tools
TOOLSPATH = $(abspath ./tools)

# Path to plads the object files 
OBJDIR := $(abspath ./build)

# configurable options
OPTIONS = -DF_CPU=48000000 -DUSB_SERIAL -DLAYOUT_US_ENGLISH -DUSING_MAKEFILE

# options needed by many Arduino libraries to configure for Teensy 3.x
OPTIONS += -D__$(MCU)__ -DARDUINO=10805 -DTEENSYDUINO=144

# use "cortex-m4" for Teensy 3.x
# use "cortex-m0plus" for Teensy LC
CPUARCH = cortex-m4
#CPUARCH = cortex-m0plus



#************************************************************************
# 
#************************************************************************

#************************************************************************
# Settings below this point usually do not need to be edited
#************************************************************************

# CFLAGS = compiler options for C and C++
CFLAGS = -Wall -g -mcpu=$(CPUARCH) -mthumb -MMD $(OPTIONS) 
# search path for headers
CFLAGS += -I. -I$(LIBDIR)
# Optimization 
CFLAGS += -Os -ffunction-sections -fdata-sections -fsingle-precision-constant
# compiler options for C++ only
CXXFLAGS = -std=gnu++14 -felide-constructors -fno-exceptions -fno-rtti

# linker options
LDFLAGS = -Os -Wl,--gc-sections,--defsym=__rtc_localtime=0 --specs=nano.specs -mcpu=$(CPUARCH) -mthumb -T$(MCU_LD)

# additional libraries to link
LIBS = -lm


# names for the compiler programs
CC = arm-none-eabi-gcc
CXX = arm-none-eabi-g++
OBJCOPY = arm-none-eabi-objcopy
SIZE = arm-none-eabi-size

# automatically create lists of the sources and objects
ifeq ($(LIBDIR),)
	C_FILES := $(shell find . -name '*.c')
else
	C_FILES := $(shell find . -name '*.c') $(shell find $(LIBDIR) -name '*.c')
endif

CPP_FILES := $(shell find . -name '*.cpp') $(shell find $(LIBDIR) -name '*.cpp')
OBJS := $(C_FILES:.c=.o) $(CPP_FILES:.cpp=.o)


# the actual makefile rules (all .o files built by GNU make's default implicit rules)

all: $(TARGET).hex


$(TARGET).elf: $(OBJS) $(MCU_LD)
	@$(CC) $(LDFLAGS) -o $@ $(OBJS) $(LIBS)

%.hex: %.elf
	@$(SIZE) $<
	@$(OBJCOPY) -O ihex -R .eeprom $< $@

fuse: $(TARGET).hex
ifneq ($(TOOLSPATH),)
		@$(TOOLSPATH)/teensy_post_compile -file=$(basename $<) -path=$(shell pwd) -tools=$(TOOLSPATH) 
		@$(TOOLSPATH)/teensy_reboot
endif
# compiler generated dependency info
-include $(OBJS:.o=.d)

install: 
ifeq ($(OS), Windows_NT)
	echo 'Install gcc-arm-none eabi manual'
else

ifeq ($(OS2), Linux)

ifneq ($(shell dpkg -l | grep -c  'gcc-arm-none-eabi'), 1)
		sudo apt-get install gcc-arm-none-eabi 
endif

endif

endif
	
remove:
ifeq ($(OS), Windows_NT)
	@echo 'uninstall gcc-arm-none eabi manual'
else

ifeq ($(shell uname), Linux)
	sudo apt-get remove gcc-arm-none-eabi 
endif

endif

setup:
	@mkdir src/	
	@mkdir build/
clean:
	@rm -f *.o *.d $(TARGET).elf $(TARGET).hex
