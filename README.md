# Sudoku Solver
Author: Will Roever
Language: Python 2.7

## Usage:
```
python solver.py file
```
Positional arguments:
```      
file        name of a CSV-formatted Sudoku template file
```

## Example input file format:

```
 , , , , , , , ,1
 , ,6, ,7, , ,3, 
 , , ,8,3,5, , , 
 , ,3,6, , , ,7, 
 , , , ,1,3, , , 
5, , , ,4, ,8, , 
1, ,5,3, ,7, ,2,6
 ,2, , , ,1,7,5, 
 , , , , , , , ,8
```

## Description:

The solver.py program takes a 9x9 CSV file and interprets it as the starting
state for a sodoku puzzle. There are two main abstractions that the program
uses in solving puzzles:
  - A BoardState class is used to represent the state of the puzzle board at
    each stage as a solution is computed. In addition to recording the
    numbers placed on the board, the BoardState keeps track of the possible
    values that empty cells may take with bit vectors (really ints, in python).
  - A Solver class which encompasses the logic of the solution process. The
    underlying algorithm is a form of the A-star guided search algorithm. At
    each stage, the algorithm seeks to take the decision (place the next
    number) that minimizes the remaining distance to the goal (number of empty
    cells) while ensuring that the Sudoku rules are satisfied. A priority
    queue is used to manage possible next steps and a "visited" set ensures
    no board state is visited twice.

There are a couple of additional optimizations the solver makes:
  - Whenever a decision is computed about where a next value should be placed
    on the board, priority is given (using a priority queue) to cells with the
    fewest possible values. This maximizes the likelihood that the decision
    results in a correct placement. For example, if we have one cell which we
    know can only take the values 1 or 2, and another which can take 1, 2, or
    3, then we have a 50% chance at choosing the correct value if we place a
    number in the first cell, vs. a 33% change for the second.
  - When the board enters a state where one or more cells has only one
    possible value, those cells are filled immediately and the board gets
    "fast-forwarded" to a state where an actual decision needs to be computed.
