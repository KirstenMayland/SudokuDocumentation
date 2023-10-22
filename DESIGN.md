# Sudoku Design Spec

*sudoku* is a standalone program that reads in the input from the user to either create a unique 9x9 sudoku puzzle or generate a solution for a 9x9 sudoku puzzle inputted through stdin.

## User Interface

One of sudoku’s interfaces is through the command-line where it must always have one argument (either 'create' or 'solve'). Sudoku’s other interface is through stdin if the command-line argument is 'solve'.

## Inputs and Outputs

Inputs: command-line arguments, either ‘create’ or ‘solve’
```
./sudoku create
```
Outputs: 

'create' will create a randomized 9x9 unfinished sudoku puzzle with a unique solution and between 40-63 missing numbers, output to stdout.

'solve' will read a valid sudoku puzzle from stdin, represented as 9 lines of text with each number separated by whitespace. The program will run through and find all possible solutions, keeping a tally of how many there are. After all have been found, the final solution's completed sudoku puzzle will be printed to stdout.

## Functional decomposition into modules

#### sudoku.c

* *main*, parses and validates given arguments, starts desired process (either ‘solve’ or ‘create’)

#### create.c

* *create*, creates an incomplete 9x9 sudoku table with a unique solution and prints it to stdout
* *switchRows*, randomizes the puzzle by switching two rows in each set of three rows and then switching two out of three sets of three rows to further randomize 
* *helpSwitch*, helper function to *switchRows* which processes the actual switching
* *removeValues*, removes a random number of values from the sudoku table while still making sure it has one unique solution
* *createRandomArray*, creates a solve order array of 9 unique values 1-9 in random order
* *makeEmptyTable*, creates an empty 9x9 sudoku puzzle (containing all 0s)

#### solve.c

* *solve*, solves a sudoku puzzle inputted through stdin and outputs a solution to stdout
* *puzzle_load*, reads a sudoku puzzle in from stdin and translates it into a 9x9 2D array

#### common.c 

* *printTable*, helper function that prints all of the values of a 2D array separated by spaces and new lines
* *cpyTbl*, changes the values within a tableDestination to exactly match those of a tableSource 
* *checkIsValid*, takes in a value and checks to see if it could work at a given position in the table
* *findNext*, finds the next empty slot in a table 
* *backtrack*, recursively iterates through a sudoku table adding values via the backtracking algorithm defined below

## Pseudocode for logic/algorithmic flow

1. Validate the command line input and call the correct function based on that input ('solve' or 'create')

### CREATE 
1. Make an array of 9 ints, containing the unique numbers 1-9, in random order
2. Make a 2D array of ints that represent indexes
3. Iterate through each spot using row and column numbers and input first num in num array [1-9] (array is randomly ordered) (the array must have a value of 0 at index 0).

    >3.1 Check if num doesn’t break the puzzle by comparing it to the values in it’s row, column and subgrid (constraint = no repeat numbers besides 0 in row, column, and subgrid set)
    >>3.1.1 If it passes, find the next open slot on the table and recursively call backtrack to repeat the process of looking for a valid number
    >>>3.1.1.1 If the backtrack reveals that multiple solutions have already been found, then break out of the loop.
    >>>
    >>>3.1.1.2 Otherwise set the current number to 0
    >>>
    >>3.1.2 If it can't find another available slot, that means it has found a solution. Increment the number of solutions and create a copy of the current table solution to store. Set current number back to 0.
4. Switch two rows in each set of three rows and then switch two out of three sets of three rows to further randomize the puzzle.
5. Once the 2D array of ints (representing the sudoku table) is created, pick a random number from 40-63 and randomly replace that specified number of values with ‘0’. After each number is removed, run solver on the resulting board to make sure that removing the tile does not create more than one solution. If it does, put the tile back and remove a different one. Make sure that at least eight of the nine distinct digits are left on the board and there are at least 18 digits on the board (to make sure it has a unique solution).
6. Print the values of the 2D array of ints to ‘stdout’ using nested for loops

### SOLVE
1. Read table in from stdin line by line and turn into 2d array int table [9][9] in memory
2. Find the first empty square (0) and take its row and column indexes to access it in the 2d array
3. While the puzzle is not complete, iterate through every empty square using these row and column indexes and input the first num (1) in num array: [0, 1, 2, 3, 4, … 9]

    >3.1 Check if num doesn’t break the puzzle by comparing it to the values in it’s row, column and subgrid (constraint = no repeat numbers besides 0 in row, column, and subgrid set)
    >>3.1.1 If it passes, find the next open slot on the table and recursively call backtrack to repeat the process of looking for a valid number
    >>>3.1.1.1 If the backtrack reveals that multiple solutions have already been found, then break out of the loop.
    >>>
    >>>3.1.1.2 Otherwise set the current number to 0
    >>>
    >>3.1.2 If it can't find another available slot, that means it has found a solution. Increment the number of solutions and create a copy of the current table solution to store. Set current number back to 0.
4. If no more empty squares and if no constraints are broken (i.e. puzzle works) then sudoku has been solved
    >4.1 When table is solved, increment solution count and check if not at the end of the numArray (9) for the last slot
    >
    >4.2 If so, backtrack to continue to find more solutions until solver can no longer progress

## Dataflow through modules


1. *main*, checks validity of arguments and then runs create or solver
2. *create*, loops through each slot on the table with backtrack using the array generated in makeRandom to create a table and then switchRows and removeValues once it has a complete table to create a puzzle and eventually calls printTable.
3. *makeRandom*, outputs an array of 9 unique values 1-9 in random order
4. *switchRows*, switches two rows in each set of three rows and then switches two out of three sets of three rows by calling *helpSwitch* to further randomize the puzzle
5. *helpSwitch*, called by *switchRows* and does the actual switching
6. *removeValues*, turns the table of values into a puzzle that needs solving
7. *solve*, calls *puzzle_load* to create a puzzle from the input and then uses *backtrack* to find all the solutions to the table. Eventually calls *printTable* to print the table
8. *backtrack*, recursively iterates through open slots on the table (finds them using *findNext*) and tries to put a valid value in that spot (*checkIsValid*). Either adds a valid value to the table or backtracks.
9. *findNext*, finds the next empty slot in a table 
10. *checkIsValid*, sees if value fits in slot
11. *puzzle_load*, loads puzzle from stdin
12. *cpyTbl*, mallocs a 2D int array with the same values as the one inputted to it
13. *makeEmptyTable*, mallocs an empty 2D int array
14. *printTable*, prints table with spaces and newlines


## Major data structures

Represents the puzzle for printing:
2D array of ints (used in both)

## Testing plan

1. Test individual functions using unit testing

2. Test the create function separately after assembly by having it create a sudoku puzzle and then solve it by hand or by an online solver to make sure that it created a solvable puzzle and count the number of blank spaces to make sure it is in the correct range.

3. Test the solver function separately after assembly by inputting sudoku puzzles that we already know the solutions to and checking the solver’s solution against our solutions.

4. *Integration testing*. Assemble the sudoku creator and solver and test it as a whole. In each case the resulting puzzles/solutions or lack there-of will be displayed based on sudoku. Error messages will also be printed so that the user knows what went wrong.

4a. Write a script to run sudoku create and direct its output into the input for sudoku solver. Run this many times to see if it comes across an error in its creation or solving of all these randomly generated puzzles.
4b. Test sudoku with invalid command-line arguments and invalid puzzles
4c. Test sudoku’s memory usage by creating and solving puzzles through valgrind.
