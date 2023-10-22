# CS50 Sudoku Project
## TESTING for sudoku

### Functionality test

Various functionality tests are conducted via the testing.sh file:

* TEST 1-6: Unit Testing
* TEST 7-10: Invalid Command-Line Arguments
* TEST 11-12: Edge Cases
* TEST 13: Fuzz Testing
* TEST 14: Valgrind Testing

#### Functionality test output

Found in *testing.out* after running `make test`

#### Unit test output

Found in *unittesting.out* after running `make unittest`

Note: for it to finish running, you need to input a puzzle to stdin, such as the one provided below

    0 0 5 0 1 8 0 2 3
    4 0 8 0 0 0 0 9 5
    7 2 0 6 0 0 0 0 0
    0 0 0 0 3 0 9 0 0
    0 6 4 0 0 0 2 0 7
    0 0 7 0 6 0 5 0 1
    0 0 0 1 5 0 8 0 0
    8 4 0 3 7 0 0 5 6
    0 5 0 0 0 0 3 7 2

### Memory test
```bash
******* TEST 14: VALGRIND TESTING *******

==21187== Command: ./sudoku create
==21187== 
==21187== Conditional jump or move depends on uninitialised value(s)
==21187==    at 0x1090E9: findNext (common.c:88)
==21187==    by 0x1091F1: backtrack (common.c:112)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187==    by 0x109211: backtrack (common.c:113)
==21187== 
Creating a unique sudoku...
0 0 0 0 0 0 0 4 0 
0 8 0 1 0 9 7 6 0 
0 0 3 7 0 2 0 0 0 
0 0 0 0 0 7 2 1 0 
0 0 0 0 1 0 0 9 0 
0 0 4 3 0 0 0 5 0 
8 0 5 0 0 1 0 3 0 
9 0 6 8 0 0 0 2 0 
0 2 0 0 3 0 0 0 5 
==21187== 
==21187== HEAP SUMMARY:
==21187==     in use at exit: 0 bytes in 0 blocks
==21187==   total heap usage: 32 allocs, 32 frees, 4,386 bytes allocated
==21187== 
==21187== All heap blocks were freed -- no leaks are possible
==21187== 
==21187== For counts of detected and suppressed errors, rerun with: -v
==21187== Use --track-origins=yes to see where uninitialised values come from
==21187== ERROR SUMMARY: 242 errors from 1 contexts (suppressed: 0 from 0)

==21209== Using Valgrind-3.13.0 and LibVEX; rerun with -h for copyright info
==21209== Command: ./sudoku solve
==21209== 
==21209== Conditional jump or move depends on uninitialised value(s)
==21209==    at 0x1090E9: findNext (common.c:88)
==21209==    by 0x1091F1: backtrack (common.c:112)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209==    by 0x109211: backtrack (common.c:113)
==21209== 
Please input the sudoku puzzle you would like to solve:

7 6 9 5 8 3 1 4 2 
5 8 2 1 4 9 7 6 3 
1 4 3 7 6 2 5 8 9 
3 9 8 6 5 7 2 1 4 
6 5 7 2 1 4 3 9 8 
2 1 4 3 9 8 6 5 7 
8 7 5 4 2 1 9 3 6 
9 3 6 8 7 5 4 2 1 
4 2 1 9 3 6 8 7 5 
Number of Solutions Found: 1
==21209== 
==21209== HEAP SUMMARY:
==21209==     in use at exit: 0 bytes in 0 blocks
==21209==   total heap usage: 93 allocs, 93 frees, 23,755 bytes allocated
==21209== 
==21209== All heap blocks were freed -- no leaks are possible
==21209== 
==21209== For counts of detected and suppressed errors, rerun with: -v
==21209== Use --track-origins=yes to see where uninitialised values come from
==21209== ERROR SUMMARY: 2 errors from 1 contexts (suppressed: 0 from 0)

```
The above output excludes the test program's normal output.

