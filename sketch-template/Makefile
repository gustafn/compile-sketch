#
# Makefile template for compiling sketches on the Intel Galileo
#
# -gustaf neumann
#
# Fill in the name of your sketch
#
SKETCH=

# Optionally, add user libraries:
EXTRA_INCLUDES=
EXTRA_VPATH=
EXTRA_TARGET=

# Example:
# EXTRA_INCLUDES=-I${LOCAL}/AdafruitBMP085/
# EXTRA_VPATH=${LOCAL}/AdafruitBMP085/
# EXTRA_TARGET=Adafruit_BMP085.o

#
# There should be no need to edit below
#

ARDUINO=/media/realroot/arduino/x86/cores/arduino/
LIBRARIES=/media/realroot/arduino/x86/libraries/
FAB=/media/realroot/arduino/x86/variants/galileo_fab_d/
LOCAL=/media/realroot/arduino-local/libraries/

INCLUDES=-I${ARDUINO} -I${FAB} -I${LIBRARIES}/Wire ${EXTRA_INCLUDES}
FLAGS=-c -g -Os -w -ffunction-sections -fdata-sections -march=i586 -m32 -DARDUINO=153

OFILES=main.o i2c.o fast_gpio_sc.o interrupt.o mux.o sysfs.o trace.o wiring_analog.o wiring_digital.o IPAddress.o Print.o RingBuffer.o Stream.o TTYUART.o UtilTime.o WMath.o WString.o variant.o Wire.o ${EXTRA_TARGET}

VPATH = ${ARDUINO}:${FAB}:${LIBRARIES}/Wire:${EXTRA_VPATH}

.c.o:
	gcc -g ${FLAGS} ${INCLUDES} $<

.cpp.o:
	g++ -g ${FLAGS} ${INCLUDES} $<

%.cpp : %.ino
	echo '#include "Arduino.h"' > $*.cpp
	cat $*.ino >> $*.cpp

all: library sketch

library: ${OFILES}
	ar rcs core.a $(OFILES)

sketch: ${SKETCH}.o core.a
	g++ -Os -Wl,--gc-sections -march=i586 -o sketch ${SKETCH}.o core.a -lm -lpthread 
	strip sketch

clean:
	rm -f *.o *.d *.a
