APP     = JBUG
CC      = as0
CFLAGS	:= 
# Options are (lower case):
#	l	   enable output listing
#	nol    disable output listing (default)
#	cre    generate cross reference table
#	s	   generate a symbol table
#	c	   enable cycle count
#	noc    disable cycle count
SRC		:= $(APP).asm
OBJ     := $(APP).s19

.PHONEY: build
build: $(APP)

$(APP): $(OBJ)

$(OBJ): $(SRC)
	$(CC) $@ $< $(CFLAGS) > %.lst
	@echo "Compiled "$@" successfully"

.PHONEY: clean
clean:
	$(RM) $(APP).s19
	$(RM) $(APP).lst
	@echo "Build artifacts removed"
