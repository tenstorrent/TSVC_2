
include ../makefiles/Makefile.defs
# Defines the list of tests
include loops.mk

COMMON_FILES=dummy.o common.o

BIN_DIR=../bin/$(COMPILER)
ASM_DIR_VEC=../asm/$(COMPILER)/vec$(SUFFIX)
ASM_DIR_NOVEC=../asm/$(COMPILER)/novec$(SUFFIX)

libs=-lm

all : $(BIN_DIR)/tsvc$(BUILD)_vec $(BIN_DIR)/tsvc$(BUILD)_novec

$(BIN_DIR)/tsvc$(BUILD)_% : tsvc_%.o $(foreach t,$(TESTS), loops/$(t)_%.o) $(COMMON_FILES)
	@mkdir -p $(BIN_DIR)
	$(CC) $^ $(libs) -o $@$(SUFFIX)

%_novec.o:%.c .FORCE
	$(CC) -I . $(flags) $(novecflags) -c -o $@ $<

%_vec.o:%.c .FORCE
	$(CC) -I . $(flags) $(vecflags) -c -o $@ $<

# Compiled with IEEE math flags to make initialisation/checksum calculations more reproducable
# Be wary that PGI doesn't respect IEEE properly with vectorisation
%.o:%.c .FORCE
	$(CC) $(flags) $(vecflags) $(ieee_math_flags) -c -o $@ $<

.PHONY: .FORCE asm
asm : $(foreach t,$(TESTS), $(ASM_DIR_VEC)/$(t).s) $(foreach t,$(TESTS), $(ASM_DIR_NOVEC)/$(t).s)

$(ASM_DIR_VEC)/%.s: loops/%.c .FORCE
	@mkdir -p $(ASM_DIR_VEC)
	$(CC) -I . $(flags) $(vecflags) -S -o $@ $<

$(ASM_DIR_NOVEC)/%.s: loops/%.c .FORCE
	@mkdir -p $(ASM_DIR_NOVEC)
	$(CC) -I . $(flags) $(novecflags) -S -o $@ $<

.PHONY: .FORCE clean
.FORCE:
	@true

clean :
	$(RM) -f *.o src/*.o *.s

