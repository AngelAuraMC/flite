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
SHAREDMODS = $(flite_MODS:%=$(LIBDIR)/lib%.so)
$(SHAREDMODS): $(LIBDIR)/libflite.so
SHAREDusenMODS = $(LIBDIR)/libflite_cmu_time_awb.so $(LIBDIR)/libflite_cmu_us_awb.so $(LIBDIR)/libflite_cmu_us_kal16.so $(LIBDIR)/libflite_cmu_us_kal.so $(LIBDIR)/libflite_cmu_us_rms.so $(LIBDIR)/libflite_cmu_us_slt.so $(LIBDIR)/libflite_cmu_indic_lang.so
$(SHAREDusenMODS): $(LIBDIR)/libflite_usenglish.so
SHAREDcmulexMODS = $(LIBDIR)/libflite_cmu_time_awb.so $(LIBDIR)/libflite_cmu_us_awb.so $(LIBDIR)/libflite_cmu_us_kal16.so $(LIBDIR)/libflite_cmu_us_kal.so $(LIBDIR)/libflite_cmu_us_rms.so $(LIBDIR)/libflite_cmu_us_slt.so  $(LIBDIR)/libflite_cmu_indic_lex.so
$(SHAREDcmulexMODS): LDFLAGS+=-L../$(LIBDIR) -lflite_cmulex
SHAREDindicMODS = $(LIBDIR)/libflite_cmu_indic_lex.so
$(SHAREDindicMODS): $(LIBDIR)/libflite_cmu_indic_lang.so
# # These are broken, don't uncomment this until you figure out why they all leak into eachother, like it makes libflite.so link itself, cmulex, indix, and usenglish, whyy?
# $(SHAREDMODS): LDFLAGS+=-L../$(LIBDIR) -lflite
# $(SHAREDusenMODS): LDFLAGS+=-L../$(LIBDIR) -lflite_usenglish
# $(SHAREDcmulexMODS): LDFLAGS+=-L../$(LIBDIR) -lflite_cmulex
# $(SHAREDindicMODS): LDFLAGS+=-L../$(LIBDIR) -lflite_cmu_indic_lang

strip_libs: $(SHAREDLIBS)
	$(STRIP) -s $(SHAREDLIBS)

copy_to_jniLibs: $(SHAREDLIBS)
	@ echo Copying libraries to jniLibs/$(ANDROID_ABI)
	@ mkdir -p $(TOP)/jniLibs/$(ANDROID_ABI)
	@ cp -t $(TOP)/jniLibs/$(ANDROID_ABI) $(SHAREDLIBS)

$(LIBDIR)/%.so: $(LIBDIR)/%.a
	@ echo android making $@ using $(LDFLAGS)
	@ echo $(SHAREDMODS)
	@ # Properly link the libraries, because stealing main/Makefile logic failed
	@ ( \
			FLAGS="$(LDFLAGS)"; \
			if echo "$(SHAREDMODS)" | grep -qw "$@"; then \
					FLAGS="$$FLAGS -L../$(LIBDIR) -lflite"; \
			fi; \
			if echo "$(SHAREDusenMODS)" | grep -qw "$@"; then \
					FLAGS="$$FLAGS -L../$(LIBDIR) -lflite_usenglish"; \
			fi; \
			if echo "$(SHAREDcmulexMODS)" | grep -qw "$@"; then \
					FLAGS="$$FLAGS -L../$(LIBDIR) -lflite_cmulex"; \
			fi; \
			if echo "$(SHAREDindicMODS)" | grep -qw "$@"; then \
					FLAGS="$$FLAGS -L../$(LIBDIR) -lflite_cmu_indic_lang"; \
			fi; \
			rm -rf shared_os; \
			mkdir shared_os; \
			rm -f $@ $@.${PROJECT_VERSION} $@.${PROJECT_SHLIB_VERSION}; \
			cd shared_os; \
			$(AR) x ../$<; \
			echo "$(CC) -shared -Wl,-soname,`basename $@` -o ../$@ *.o $$FLAGS"; \
			$(CC) -shared -Wl,-soname,`basename $@` -o ../$@ *.o $$FLAGS; \
			cd ../; \
			rm -rf shared_os; \
		)

$(LIBDIR)/%.a: $(FULLOBJS)
	@ (cd $(OBJDIR) && $(AR) crv ../../../$(LIBDIR)/lib$(LIBNAME).a $(OBJS))
	@ $(RANLIB) $(LIBDIR)/lib$(LIBNAME).a
endif

