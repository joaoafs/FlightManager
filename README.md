# Flight Manager
#### @IST - IAED Project 2021

> The intention is to build a flight management system between airports. To do so, your system must allow the definition of of airports and flights, as well as your query. You can read all about the project [here](https://github.com/joaoafs/FlightManager/blob/main/Statement.md).


###### Program Execution

How to compile: 

```
     gcc -Wall -Wextra -Werror -ansi -pedantic -o proj *.c
```
How to run: 

```
     ./proj < test.in > test.myout
```

Later you can compare your output (`*.myout`) with the predicted output (`*.out`) using the `diff` command,

```
     $ diff test.out test.myout
```

To test your program you can follow the steps above or use the `make` command in the `tests/` folder. in the `tests/` folder.
To run all the tests with *valgrind* you can run `make valgrind` in the `tests/` folder.
