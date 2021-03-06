SHELL     = /bin/bash
PERL      = perl
CHECKOUT  = checkout_file
CECHO     = $(PERL) -e'use Term::ANSIColor; print color(shift); print @ARGV, color("reset"), "\n"'
BLUE      = $(CECHO) "blue"

CFLAGS    = -fshort-enums -I../../tests/include/gccinc -I../../tests/include/include -I../../tests/include/perlinc

SIZEOF    = ../../tests/include/sizeof.pl
OFFSETOF  = ../../tests/include/offsetof.pl

all :: install clean

../../blib:
	@cd ../.. && $(PERL) Makefile.PL && make

module :: ../../blib

sizeof.pl: sizeof.txt mksizeof.pl
	@$(BLUE) "making sizeof.pl"
	@$(PERL) -ne'/^#/ or exit; print' <$(SIZEOF) >sizeof.pl
	@$(PERL) mksizeof.pl <sizeof.txt >>sizeof.pl

install: sizeof.pl
	@if diff sizeof.pl $(SIZEOF) >/dev/null ; then                  \
	  $(CECHO) "bold blue" "$(SIZEOF) is unchanged, skip install";  \
	else                                                            \
	  $(CHECKOUT) $(SIZEOF);                                        \
	  $(CECHO) "bold red" "installing $(SIZEOF)";                   \
	  cp sizeof.pl $(SIZEOF);                                       \
	fi

clean:
	@$(BLUE) "cleaning up"
	@rm -f *.o mksizes mksizes.c sizeof.pl sizeof.txt \
	           mkoffsets mkoffsets.c offsetof.pl offsetof.txt

sizeof.txt: mksizes
	@$(BLUE) "making sizeof.txt"
	@./mksizes >sizeof.txt

mksizes: mksizes.c
	@$(BLUE) "making mksizes"
	@$(CC) $(CFLAGS) -o mksizes mksizes.c

mksizes.c: module mksizes.pl
	@$(BLUE) "making mksizes.c"
	@$(PERL) -I../../blib/lib -I../../blib/arch mksizes.pl > mksizes.c

offsetof.pl: offsetof.txt mkoffsetof.pl
	@$(BLUE) "making offsetof.pl"
	@$(PERL) -ne'/^#/ or exit; print' <$(OFFSETOF) >offsetof.pl
	@$(PERL) mkoffsetof.pl <offsetof.txt >>offsetof.pl

offsetof.txt: mkoffsets
	@$(BLUE) "making offsetof.txt"
	@./mkoffsets >offsetof.txt

mkoffsets: mkoffsets.c
	@$(BLUE) "making mkoffsets"
	@$(CC) $(CFLAGS) -o mkoffsets mkoffsets.c

mkoffsets.c: module mkoffsets.pl
	@$(BLUE) "making mkoffsets.c"
	@$(PERL) -I../../blib/lib -I../../blib/arch mkoffsets.pl > mkoffsets.c
