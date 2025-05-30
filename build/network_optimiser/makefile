##########################################################################
 # Copyright 2023 Comcast Cable Communications Management, LLC
 #
 # Licensed under the Apache License, Version 2.0 (the "License");
 # you may not use this file except in compliance with the License.
 # You may obtain a copy of the License at
 #
 # http://www.apache.org/licenses/LICENSE-2.0
 #
 # Unless required by applicable law or agreed to in writing, software
 # distributed under the License is distributed on an "AS IS" BASIS,
 # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 # See the License for the specific language governing permissions and
 # limitations under the License.
 #
 # SPDX-License-Identifier: Apache-2.0
##########################################################################

include ../makefile.inc

#
# program
#

PROGRAM = $(INSTALLDIR)/bin/onewifi_em_network_optimiser

INCLUDEDIRS = \
	-I$(ONEWIFI_EM_HOME)/inc \
	-I$(ONEWIFI_EM_HOME)/src/utils \
	-I$(ONEWIFI_HAL_INTF_HOME) \
	-I$(ONEWIFI_HOME)/source/utils \
	-I$(ONEWIFI_HOME)/include \
    	-I$(ONEWIFI_BUS_LIB_HOME)/inc \
    	-I$(ONEWIFI_HOME)/lib/log \
    	-I$(ONEWIFI_HOME)/lib/ds \
    	-I$(ONEWIFI_HOME)/source/platform/linux \
    	-I$(ONEWIFI_HOME)/source/platform/common \
    	-I$(ONEWIFI_HOME)/source/platform/linux/he_bus/inc \
        -I$(ONEWIFI_EM_HOME)/src/util_crypto \
        -I$(ONEWIFI_HOME)/source/ccsp
ifeq ($(WITH_SAP), 1)
INCLUDEDIRS += -I$(AL_SAP_HOME)/include
endif

$(info The value of VARIABLE is $(ONEWIFI_HAL_INTF_HOME))

CXXFLAGS += $(INCLUDEDIRS) -g -DEASY_MESH_NODE -DEM_APP -Wall -Wextra -Wpointer-arith -Wcast-qual -Wcast-align -Wstrict-aliasing -fno-common -Wctor-dtor-privacy -Wold-style-cast -Woverloaded-virtual -Wsign-promo -Wstrict-null-sentinel -fstack-protector-strong -D_FORTIFY_SOURCE=2 -fPIE -pie -ftrapv -Wformat=2 -Wformat-security -Wuninitialized -Winit-self -Wconversion -Wsign-conversion -Weffc++ -Wno-unused-parameter -std=c++17 -fsanitize=address -fsanitize=undefined #-Werror -O2
ifeq ($(WITH_SAP), 1)
CXXFLAGS += -DAL_SAP
endif
LDFLAGS += $(LIBDIRS) $(LIBS) -fsanitize=address -fsanitize=undefined

LIBDIRS = \
	-L$(INSTALLDIR)/lib \
    -L$(ONEWIFI_HOME)/install/lib/
ifeq ($(WITH_SAP), 1)
LIBDIRS += -L$(AL_SAP_HOME)/
endif

LIBS = -lm -lpthread -ldl -lcjson -luuid -lssl -lcrypto -lwebconfig -lhebus
ifeq ($(WITH_SAP), 1)
LIBS += -lalsap
endif

GENERIC_SOURCES = $(ONEWIFI_HOME)/source/utils/collection.c \
    $(ONEWIFI_EM_SRC)/util_crypto/aes_siv.c \
    $(ONEWIFI_HOME)/lib/common/util.c \
    $(ONEWIFI_HOME)/source/platform/linux/bus.c \

SOURCES = $(wildcard $(ONEWIFI_EM_SRC)/network_optimiser/*.cpp) \

OBJECTS = $(SOURCES:.cpp=.o)
GENERIC_OBJECTS = $(GENERIC_SOURCES:.c=.o) 
ALLOBJECTS = $(OBJECTS) $(GENERIC_OBJECTS)

all: $(BUS_LIBRARY) $(PROGRAM)

$(PROGRAM): $(ALLOBJECTS)
	$(CXX) -o $@ $(ALLOBJECTS) $(LDFLAGS)

$(GENERIC_OBJECTS): %.o: %.c
	$(CC) $(CXXFLAGS) -o $@ -c $<

$(OBJECTS): %.o: %.cpp
	$(CXX) $(CXXFLAGS) -o $@ -c $<

# Clean target: "make -f Makefile.Linux clean" to remove unwanted objects and executables.
#

clean:
	$(RM) $(ALLOBJECTS) $(PROGRAM)

#
# Run target: "make -f Makefile.Linux run" to execute the application
#             You will need to add $(VARIABLE_NAME) for any command line parameters 
#             that you defined earlier in this file.
# 

run:
	./$(PROGRAM) 

