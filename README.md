# CS50 Spring 2022, Project-McKaba

## Team Members
- Anand McCoole (@anandmccoole)
- Amethyst McKenzie (@amethystmckenzie)
- Brett McGee (@bmcgee9)
- Kirsten Mayland (@KirstenMayland)

## sudoku

*sudoku* is a standalone program that reads in the input from the user to either create a 9x9 sudoku puzzle or solve a 9x9 sudoku puzzle imputed through stdin.

## Usage

``` bash
$ ./sudoku version
```

 * where `version` is either "create" or "solve"

## Assumptions

Format of table read from ```stdin``` and printed to ```stdout```:
 * The grid is represented as 9 lines of text
 * Each line contains 9 integers that range from 0 to 9, separated by a white space
 * 0 represents a missing number in the grid

## Files

* `Makefile` - compilation procedure
* `sudoku.c` - the implementation
* `IMPLEMENTATION.md` - details the implementation of crawler
* `DESIGN.md` - design spec
* `TESTING.md` - testing spec
* `common.h` - the interface of the common module
* `common.c` - the implementation of the common module
* `solve.h` - the interface of the solve module
* `solve.c` - the implementation of the solve module
* `create.h` - the interface of the create module
* `create.c` - the implementation fo the create module
* `testing.sh` - runs script containing many test cases
* `testing.out` - contains the output of the testing script

## Compilation
To build, run `make`.

To clean up, run `make clean`

To test, run `make test`

To valgrind, run `make valgrind`

## Testing

Detailed in `TESTING.md`

## Credit

Conceptual plan for how to implement sudoku create came from Hina Sharma's answer to the question "How are Sudoku puzzles made?" on Quora (https://www.quora.com/How-are-Sudoku-puzzles-made)

Additional conceptual ideas for how to implement sudoku create came from Doc Brown's answer to the question "How to generate Sudoku boards with unique solutions" on StackOverflow (https://stackoverflow.com/questions/6924216/how-to-generate-sudoku-boards-with-unique-solutions)