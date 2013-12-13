LIBS =
CC = g++
CFLAGS =-g -Wall -Os -shared -fPIC
CFLAGS_EXE =-g -Wall

.PHONY: default all clean

TEST_TARGETS = all_markers_test builder_test configuration_test descender_test \
    interval_test mutation_test node_test simulator_test


SHARED_MINUS = builder.o builder_events.o configuration.o descender.o \
	       epochs.o interval.o marker.o micro_satellite_marker.o \
	       node.o retired_interval.o simulator.o snp_marker.o \
	       trait_marker.o

SHARED = $(SHARED_MINUS) dist_funcs.o


HEADERS = $(wildcard *.hh)

%.o: %.cc $(HEADERS)
	$(CC) $(CFLAGS) -c $< -o $@

default: $(SHARED)
	rm -f libcoasim.a
	ar cru libcoasim.a $(SHARED)
	ranlib libcoasim.a

all_markers_test: all_markers_test.o $(SHARED)
	$(CC) $(CFLAGS_EXE) $(SHARED) all_markers_test.o -Wall $(LIBS) -o $@
builder_test: builder_test.o $(SHARED)
	$(CC) $(CFLAGS_EXE) $(SHARED) builder_test.o -Wall $(LIBS) -o $@
configuration_test: configuration_test.o $(SHARED)
	$(CC) $(CFLAGS_EXE) $(SHARED) configuration_test.o -Wall $(LIBS) -o $@
descender_test: descender_test.o $(SHARED)
	$(CC) $(CFLAGS_EXE) $(SHARED) descender_test.o -Wall $(LIBS) -o $@
interval_test: interval_test.o $(SHARED)
	$(CC) $(CFLAGS_EXE) $(SHARED) interval_test.o -Wall $(LIBS) -o $@
# Note: mutation_test uses a different set of dist_funcs
mutation_test: mutation_test.o test_dist_funcs.o $(SHARED_MINUS)
	$(CC) $(CFLAGS_EXE) $(SHARED_MINUS) test_dist_funcs.o mutation_test.o -Wall $(LIBS) -o $@
node_test: node_test.o $(SHARED)
	$(CC) $(CFLAGS_EXE) $(SHARED) node_test.o -Wall $(LIBS) -o $@
simulator_test: simulator_test.o $(SHARED)
	$(CC) $(CFLAGS_EXE) $(SHARED) simulator_test.o -Wall $(LIBS) -o $@

all: default $(TEST_TARGETS)

clean:
	-rm -f *.o
	-rm -f libcoasim.a $(TEST_TARGETS)

run_tests: $(TEST_TARGETS)
	# alternative:
	# $(foreach target,$(TEST_TARGETS),./$(target);)
	for test in $(TEST_TARGETS); do \
	    echo $$test ; \
	    ./$$test ; \
	done
