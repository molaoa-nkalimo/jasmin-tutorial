include ../Makefile.base

JAZZ_INCLUDES = comp_and_swap.jinc msort.jinc

.PHONY: all run run-solution ct clean

all: sort.s test

run: test
	./test

run-solution: test_solution
	./test_solution

# Checks that the sort is constant time (see section 4 of TUTORIAL.md).
ct:
	jasmin-ct --doit sort.jazz

sort.s: sort.c
	$(CC) -O3 -S -o $@ $<

%_jazz.s: %.jazz $(JAZZ_INCLUDES)
	$(JASMINC) $(JASMIN_FLAGS) -o $@ $<

test: test.c sort.s sort_jazz.s
	$(CC) $(CFLAGS) -o $@ $^

test_solution: test.c sort.s sort_solution_jazz.s
	$(CC) $(CFLAGS) -o $@ $^

clean:
	rm -f sort.s *_jazz.s test test_solution
