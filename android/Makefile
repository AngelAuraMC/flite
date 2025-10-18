###########################################################################
##                                                                       ##
##                  Language Technologies Institute                      ##
##                     Carnegie Mellon University                        ##
##                         Copyright (c) 2010                            ##
##                        All Rights Reserved.                           ##
##                                                                       ##
##  Permission is hereby granted, free of charge, to use and distribute  ##
##  this software and its documentation without restriction, including   ##
##  without limitation the rights to use, copy, modify, merge, publish,  ##
##  distribute, sublicense, and/or sell copies of this work, and to      ##
##  permit persons to whom this work is furnished to do so, subject to   ##
##  the following conditions:                                            ##
##   1. The code must retain the above copyright notice, this list of    ##
##      conditions and the following disclaimer.                         ##
##   2. Any modifications must be clearly marked as such.                ##
##   3. Original authors' names are not deleted.                         ##
##   4. The authors' names are not used to endorse or promote products   ##
##      derived from this software without specific prior written        ##
##      permission.                                                      ##
##                                                                       ##
##  CARNEGIE MELLON UNIVERSITY AND THE CONTRIBUTORS TO THIS WORK         ##
##  DISCLAIM ALL WARRANTIES WITH REGARD TO THIS SOFTWARE, INCLUDING      ##
##  ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS, IN NO EVENT   ##
##  SHALL CARNEGIE MELLON UNIVERSITY NOR THE CONTRIBUTORS BE LIABLE      ##
##  FOR ANY SPECIAL, INDIRECT OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES    ##
##  WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN   ##
##  AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION,          ##
##  ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF       ##
##  THIS SOFTWARE.                                                       ##
##                                                                       ##
###########################################################################
##                                                                       ##
##  Author: Alok Parlikar (aup@cs.cmu.edu)                               ##
##                                                                       ##
##  Android support for Flite                                            ##
##                                                                       ##
##  Android support for Flite is now in a different project              ##
##                                                                       ##
##  git clone https://github.com/happyalu/Flite-TTS-Engine-for-Android   ##
##                                                                       ##
##  and follow instructions in Flite-TTS-Engine-for-Android/README.rst   ##
##                                                                       ##
###########################################################################
TOP = ..
DIRNAME = android
BUILD_DIRS = 
ALL_DIRS = $(BUILD_DIRS) 
H = 
SRCS = 
OBJS = $(SRCS:.c=.o)
IMGS = 
OTHER = 
FILES = Makefile README

BUILD_DIRS = ../include ../src ../lang
VOICES=$(VOXES)
VOICELIBS=$(VOICES:%=flite_%)
flite_MODS = $(VOICELIBS) $(LANGS:%=flite_%) $(LEXES:%=flite_%)
flite_LIBS = $(flite_MODS) flite
ALL = copy_to_jniLibs

include $(TOP)/config/common_make_rules
ifdef SHFLAGS
OTHER_BUILD_DIRS = ../main ../tools
# Stub target because it's not supposed to do anything here
copy_to_jniLibs:
else
# strip removes a trailing space
ANDROID_ABI = $(strip\
								$(if $(filter armv7,$(TARGET_CPU)),armeabi-v7a,\
								$(if $(filter aarch64,$(TARGET_CPU)),arm64-v8a,\
								$(if $(filter x86,$(TARGET_CPU)),x86,\
								$(if $(filter x86_64,$(TARGET_CPU)),x86_64,\
 								$(if $(filter mips,$(TARGET_CPU)),mips,\
 								$(if $(filter armv5te,$(TARGET_CPU)),armeabi,\
 								$(error Unknown TARGET_CPU: $(TARGET_CPU)))))))))
LOCAL_CLEAN = $(wildcard $(TOP)/jniLibs/$(ANDROID_ABI)/*.so)
SHAREDARLIBS = $(flite_LIBS:%=$(LIBDIR)/lib%.a)
SHAREDLIBS = $(SHAREDARLIBS:%.a=%.so)

strip_libs: $(SHAREDLIBS)
	$(STRIP) -s $(SHAREDLIBS)

copy_to_jniLibs: strip_libs
	@ echo Copying libraries to jniLibs/$(ANDROID_ABI)
	@ mkdir -p $(TOP)/jniLibs/$(ANDROID_ABI)
	@ cp -t $(TOP)/jniLibs/$(ANDROID_ABI) $(SHAREDLIBS)

$(LIBDIR)/%.a: $(FULLOBJS)
	@ (cd $(OBJDIR) && $(AR) crv ../../../$(LIBDIR)/lib$(LIBNAME).a $(OBJS))
	@ $(RANLIB) $(LIBDIR)/lib$(LIBNAME).a

$(LIBDIR)/%.so: $(LIBDIR)/%.a
	@ echo android making $@
	@ rm -rf shared_os && mkdir shared_os
	@ rm -f $@ $@.${PROJECT_VERSION} $@.${PROJECT_SHLIB_VERSION}
	@ (cd shared_os && $(AR) x ../$<)
	@ (cd shared_os && $(CC) -shared -Wl,-soname,`basename $@` -o ../$@ *.o $(LDFLAGS))
	@ rm -rf shared_os
endif

