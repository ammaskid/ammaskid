.PHONY: all clean

all: main

main: main.o
	gcc $^ -o $@

%.o: %.c
	gcc $< -o $@ -c

clean: 
	rm -f *.o main trash