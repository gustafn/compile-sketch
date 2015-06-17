#
# Makefile template for compiling sketches on the Intel Galileo
#
# -gustaf neumann
#
# Fill in the name of your sketch
#
SKETCH=

# Optionally, add additional includes and user libraries:
EXTRA_INCLUDES=
EXTRA_VPATH=
EXTRA_TARGET=

#
# Example for adding local user library (AdafruitBMP085)
#
#EXTRA_INCLUDES=-I${LOCAL}/Adafruit_BMP085/
#EXTRA_VPATH=${LOCAL}/Adafruit_BMP085/
#EXTRA_TARGET=Adafruit_BMP085.o

#
# Example for adding shipped library LiquidCrystal with sample sketches:
#
# EXTRA_INCLUDES=-I${LIBS}/LiquidCrystal/src/
# EXTRA_VPATH=${LIBS}/LiquidCrystal/src/
# EXTRA_TARGET=LiquidCrystal.o

VOLUME=/media/realroot/arduino/
VOLUME=/home/root/src/

#HW=${VOLUME}/arduino/x86/
HW=${VOLUME}/i586-uclibc/

# Galileo 1
FAB=${HW}/variants/galileo_fab_d/
# Galileo Gen2
FAB=${HW}/variants/galileo_fab_g/

#
# There should be no need to edit below
#

ARDUINO=${HW}/cores/arduino/
LIBRARIES=${HW}/libraries/
LOCAL=${VOLUME}/arduino-local/libraries/

INCLUDES=-I${ARDUINO} -I${FAB} -I${LIBRARIES}/Wire/src ${EXTRA_INCLUDES}
FLAGS=-c -g -Os -w -ffunction-sections -fdata-sections -march=i586 -m32 -DARDUINO=153

OFILES=main.o i2c.o interrupt.o mux.o sysfs.o trace.o \
	fast_gpio_common.o fast_gpio_sc.o fast_gpio_nc.o \
	IPAddress.o Print.o RingBuffer.o \
	Stream.o TTYUART.o UtilTime.o WMath.o WString.o variant.o \
	wiring_digital.o wiring_analog.o \
	SPI.o Wire.o \
	${EXTRA_TARGET}

VPATH = ${ARDUINO}:${FAB}:${LIBRARIES}/SPI/src:${LIBRARIES}/Wire/src:${EXTRA_VPATH}

.c.o:
	gcc -g ${FLAGS} ${INCLUDES} $<

.cpp.o:
	g++ -g ${FLAGS} ${INCLUDES} $<

%.cpp : %.ino
	echo '#include "Arduino.h"' > $*.cpp
	cat $*.ino >> $*.cpp

all: library sketch

library: ${OFILES}
	ar rcs core.a ${OFILES}

sketch: ${SKETCH}.o core.a
	g++ -Os -Wl,--gc-sections -march=i586 -o sketch ${SKETCH}.o core.a -lm -lpthread 
	strip sketch

clean:
	rm -f *.o *.d *.a
