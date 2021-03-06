#**************************************************************************************************
#
#   raylib - Basic Game
#
#   makefile to compile basic game
#
#   Copyright (c) 2014 Ramon Santamaria (Ray San - raysan@raysanweb.com)
#    
#   This software is provided "as-is", without any express or implied warranty. In no event 
#   will the authors be held liable for any damages arising from the use of this software.
#
#   Permission is granted to anyone to use this software for any purpose, including commercial 
#   applications, and to alter it and redistribute it freely, subject to the following restrictions:
#
#     1. The origin of this software must not be misrepresented; you must not claim that you 
#     wrote the original software. If you use this software in a product, an acknowledgment 
#     in the product documentation would be appreciated but is not required.
#
#     2. Altered source versions must be plainly marked as such, and must not be misrepresented
#     as being the original software.
#
#     3. This notice may not be removed or altered from any source distribution.
#
#**************************************************************************************************

# define raylib platform (by default, compile for RPI)
# Other possible platform: PLATFORM_DESKTOP, PLATFORM_WEB, PLATFORM_RPI
PLATFORM ?= PLATFORM_WEB

# define compiler: gcc for C program, define as g++ for C++
ifeq ($(PLATFORM),PLATFORM_WEB)
    # define emscripten compiler
    CC = emcc
else
    # define default gcc compiler
    CC = gcc
endif

# define compiler flags:
#  -O2         defines optimization level
#  -Wall       turns on most, but not all, compiler warnings
#  -std=c99    use standard C from 1999 revision
ifeq ($(PLATFORM),PLATFORM_RPI)
    CFLAGS = -O1 -Wall -std=gnu99 -fgnu89-inline
else
    CFLAGS = -O1 -Wall -std=c99
endif

ifeq ($(PLATFORM),PLATFORM_WEB)
    CFLAGS = -O1 -Wall -std=c99 -s USE_GLFW=3 -s ASSERTIONS=1 --preload-file resources
    #-s ALLOW_MEMORY_GROWTH=1   # to allow memory resizing
    #-s TOTAL_MEMORY=16777216   # to specify heap memory size (default = 16MB)
endif

#CFLAGSEXTRA = -Wextra -Wmissing-prototypes -Wstrict-prototypes

# define any directories containing required header files
ifeq ($(PLATFORM),PLATFORM_RPI)
    INCLUDES = -I. -I../../src -I/opt/vc/include -I/opt/vc/include/interface/vcos/pthreads
else
    INCLUDES = -I. -I../../src
endif

# define library paths containing required libs
ifeq ($(PLATFORM),PLATFORM_WEB)
    LFLAGS = -L.
else
    LFLAGS = -L. -L../../src -L/opt/vc/lib
endif

# define any libraries to link into executable
# if you want to link libraries (libname.so or libname.a), use the -lname
ifeq ($(PLATFORM),PLATFORM_RPI)
    # libraries for Raspberry Pi compiling
    # NOTE: OpenAL Soft library should be installed (libopenal1 package)
    LIBS = -lraylib -lGLESv2 -lEGL -lpthread -lrt -lm -lbcm_host -lopenal
else
    # libraries for Windows desktop compiling
    # NOTE: GLFW3 and OpenAL Soft libraries should be installed
    LIBS = -lraylib -lglfw3 -lglew32 -lopengl32 -lopenal32 -lgdi32
endif

ifeq ($(PLATFORM),PLATFORM_WEB)
    LIBS = ../../src/libraylib.bc
endif

# define additional parameters and flags for windows
ifeq ($(PLATFORM),PLATFORM_DESKTOP)
    # resources file contains windows exe icon
    # -Wl,--subsystem,windows hides the console window
    WINFLAGS = ../../src/resources -Wl,--subsystem,windows
endif

ifeq ($(PLATFORM),PLATFORM_WEB)
    EXT = .html
endif

# typing 'make' will invoke the first target entry in the file,
# in this case, the 'default' target entry is qidv_raylib
default: raylib_demo

# compile qidv_raylib
raylib_demo: raylib_demo.c
	$(CC) -o $@$(EXT) $< $(CFLAGS) $(INCLUDES) $(LFLAGS) $(LIBS) -D$(PLATFORM) $(WINFLAGS)

# clean everything
clean:
ifeq ($(PLATFORM),PLATFORM_RPI)
	rm -f *.o
#	find . -executable -delete
else
ifeq ($(PLATFORM),PLATFORM_WEB)
	del *.html *.js
else
	del *.o *.exe
endif
endif
	@echo Cleaning done

# instead of defining every module one by one, we can define a pattern
# this pattern below will automatically compile every module defined on $(OBJS)
#%.exe : %.c
#	$(CC) -o $@ $< $(CFLAGS) $(INCLUDES) $(LFLAGS) $(LIBS) -D$(PLATFORM)
