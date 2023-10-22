# Project McKaba - Sudoku Implementation Spec

Implementation Spec will look at:

* Detailed pseudo code for each of the objects/components/functions
* Definition of detailed APIs, interfaces, function prototypes and their parameters
* Data structures (e.g., struct names and members)

sudoku was written in C, but can be run on any machine that is capable of compiling C files and running executables.


### solve.c

#### solve()

* Solves a 9x9 sudoku puzzle inputted to stdin and prints a solution to stdout

Parameters: int *countSolutions

1. initialise table 2D array
2. load user inputted table in from stdin using puzzle_load()
3. initialise solutionTable 2D array
4. recursively solve table starting at row = 0 column = 0, using backtrack()
6. create solve order array of 10 integers 1-9 in ascending order
7. print solved table to stdout

Returns: int 0 upon success, otherwise upon error

#### puzzleLoad()

* Loads a 9x9 sudoku puzzle inputted through stdin into a 9x9 int 2D array

Parameters: int puzzle[9][9]

1. initialise integers row and col to 0
2. prompt user to input sudoku puzzle to be solved
3. read the inputted sudoku table in from stdin using freadwordp(), reading number by number
4. validate that current number is in valid position on the sudoku table by checking row and col values
5. convert string number into an integer
6. input number into puzzle[row][col]
7. increment row and column position for next number to be inserted into
8. free allocated memory to string number 
8. when all numbers have been read in, print newline to stdout

Returns: void

### create.c

#### create()

* Creates a unique 9x9 sudoku puzzle and prints it to stdout

Parameters: -

1. create table 2D array using makeEmptyTable()
2. initialise solutionTable 2D array
3. set numSol = 0
4. recursively solve empty table starting at row = 0 column = 0, using backtrack()
5. randomly switch rows in table using switchRows()
6. remove between 40-63 values from table using removeValues()
7. create random solve order array of 10 integers using createRandomArray()
8. print sudoku puzzle to stdout using printTable()

Returns: int 0 upon sucess, otherwise upon error

#### switchRows()

* Randomizes a sudoku puzzle by switching rows within each row of 3x3 subgrids, as well as switching subgrid rows

Parameters: int table[9][9]

1. initialise int a and int b as 0
2. set a and b to a random number between 0 and 2
3. switch rows a and b in top row of table subgrids using helpSwitch()
4. set a and b to a random number between 3 and 5
5. switch rows a and b in middle row of table subgrids using helpSwitch()
6. set a and b to a random number between 6 and 8
7. switch rows a and b in bottom row of table subgrids using helpSwitch()

8. set a and b to a random number between 0 and 8
9. decrement a and b until divisible by 3
10. for loop to iterate int i 3 times 
11. switch rows a+i and b+i using helpSwitch(), effectively switching rows of subgrids on the table

Returns: void

#### helpSwitch()

* Helper function used in switchRows()

Parameters: int table[9][9], int row1, int row2

1. validate row numbers
2. initialise int temp[9] as a temporary memory storage for rows being switched
3. for loop iterating int i 9 times
    * set temp[i] = table[row1][i]
    * set table[row1][i] = table[row2][i]
    * set table[row2][i] = temp[i]

Returns: void

#### removeValues()

* Removes a random number between 40-63 values from a completed sudoku puzzle, and ensures that each time a value is removed, the puzzle still has 1 unique solution

Parameters: int puzzle[9][9]

1. create forBacktrackTable 2D array using makeEmptyTable()
2. create solutionTable 2D array using makeEmptyTable()

3. set int numTaken to a random number between 40 and 63, which will be number of clues removed
4. initialise count = 0 and checkInfinite = 0

5. while loop that runs while count is less than numTaken and checkInfinite is less than 21
    * set int x to a random number 1-9
    * set int y to a random number 1-9
    * if puzzle[x][y] is not empty (0):
        * save puzzle[x][y] value in temp variable
        * remove value from puzzle (set puzzle[x][y] to empty (0))
        * copy puzzle to forBacktrackTable using cpyTbl()
        * backtrack through forBacktrackTable, passing it an address to numSol (finds number of solutions when clue is removed) 
        * if numSol is 1 (puzzle maintains having a unique solution), increment count and set checkInfinite to 0
        * otherwise, set table[x][y] to temp (adding clue back in) and increment checkInfinite

Returns: void

#### createRandomArray()

* Creates an array of 10 integers, where [0] = 0, and [1-9] are 1-9 in randomized order

Parameters: int array[10]

1. set first element in array to 0
2. create new empty set of values 
3. initialise int added variable to 1
4. while added is less than 10
    * get a random number tempNum between 1-9
    * allocate memory to string variable stringOfNum
    * insert tempNum into stringOfNum using sprintf()
    * if stringOfNum successfully inserts into set of values (i.e. value isn't already in array)
        * add tempNum to array, increment added
    * free memory allocated to stringOfNum
5. delete set of values

Returns: void

#### makeEmptyTable()

* Creates a 9x9 2D array of integers filled with 0s

Parameters: int table[9][9]

1. nested for loop iterating through each sqaure in 9x9 table
    * set value of current square to 0

Returns: void

### common.c 

#### printTable()

* Prints a sudoku 2D array of ints to stdout, specified by a solve order array of 10 integers

Parameters: int solveOrder[10], int table[9][9]

1. nested for loop iterating through every square in table
    * use value of table[row][col] as the index of the value in solveOrder to print out to stdout
    * flush buffer output to stdout
3. after loop terminates, print newline and flush buffer output to stdout

Returns: void

#### cpyTbl()

* Sets each value in tableDestination to the corresponding value in tableSource 

Parameters: int tableSource[9][9], int tableDestination[9][9]

1. nested for loop to iterate through each square in tableSource
    * set tableDestination value of current square to corresponding tableSource value

Returns: void

#### checkIsValid()

* Checks whether a sudoku puzzle's constraints will be broken if given value is inputted into given slot in puzzle

Parameters: int table[9][9], int value, int row, int col

1. for loop iterating int i 9 times through given row
    * if i is equal to given column (i.e. checking against current position), continue for loop from next i
    * if table[row][i] is equal to value, return false (value already exists in row)

2. for loop iterating int j 9 times through given column
    * if j is equal to given row (i.e. checking against current position), continue for loop from next j
    * if table[j][column] is equal to value, return false (value already exists in column)

3. initialise sqRow position by decrementing row until divisible by 3
4. initialise sqCol position by decrementing column until divisible by 3

5. nested for loop iterating through each square in 3x3 subgrid, starting from top right slot position (sqRow, sqCol)
    * if i and k are equal to current row and column position (checking against current position), continue from next square
    * if table[i][k] is equal to value, return false (value already exists in subgrid)

6. return true (if reaches the end, inputted value doesn't break sudoku puzzle constraints)

Returns: true upon success, false upon failure

#### findNext()

* Finds the next slot in the sudoku puzzle that is empty, and updates the nextR and nextC values accordingly

Parameters: int table[9][9], int currR, int currC, int *nextR, int *nextC

1. set nextR and nextC to currR and currC
2. while table[*nextR][*nextC] is not empty (0)
    * if nextC is 8 (at the last column)
        * increment nextR
        * set nextC to 0 (first column)
    * otherwise, increment nextC
3. if position of nextC and nextR are valid
    * return true
4. otherwise, return false

Returns: true upon finding valid empty square, false otherwise

#### backtrack() 

* Recursively iterates through sudoku table, checking each slot for possible correct numbers, moving forward if a number works and if not resets values back to 0 as it moves backwards. Keeps track of a solution and the number of solutions.

Parameters: int row, int col, int table[9][9], int solutionTable[9][9], int *numSolution

1. validate row and column positions to ensure function is processing a valid square
2. for loop iterating int i 9 times through 1-9 inclusive
    * if i value inserted at current row and column position would not break sudoku constraints (using checkIsValid())
        * set table[row][col] to i
        * find next empty square using findNext(), and update those values to nextR and nextC
        * backtrack from nextR and nextC
        * if numSolution is greater than 1
            * break
        * otherwise set table[row][col] to 0
    * otherwise if no more empty squares
        * copy table to solutionTable
        * set table[row][col] to 0
        * increment numSolutions
        * break

Returns: void

### Data structures

2D array of integers -  representation of a 9x9 sudoku puzzle


### Error Handling:
An error message is printed out to stderr everytime there is an error in the code. The only time that we return a non-zero value is if there is an error within create or solve.