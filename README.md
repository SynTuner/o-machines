The construction is based on Harrison's construction of plain machines. The following features are added to carry out the relativized computation:

- An oracle tape, which is represented by a function from integers to {One, Blank}.

- A halting stage, which allows users to limit the steps of computations.

- A use function to trace the usage of oracle tape.

Other conventions of oracle machines follow Soare (1987).

To get started, load the code into OCaml's toplevel:

    # #use "oracle machine.ml";;

Write a function to define oracle tape, for example:

    # let oracle x = if x = 2 then One else Blank;;

Write an o-machine program with the command form of **(current state, oracle, symbol) |-> (next state, action, direction)** by replacing the following command set in the list with your own one:

    # let prog = itlist (fun m -> m)
    [(1,One,One) |-> (1, One,Right); (1,Blank,One) |-> (1, One,Right);
    (1, One, Blank) |-> (2, Blank, Left); (1, Blank, Blank) |-> (2, Blank, Left);
    (2,One, One) |-> (3,Blank,Left); (2,Blank, One) |-> (4,Blank,Left);
    (3,One, One) |-> (3, One,Left); (3,Blank, One) |-> (3,One,Left);
    (3,One, Blank) |-> (5, Blank,Right); (3,Blank, Blank) |-> (5, Blank,Right);
    (5,One, One) |-> (0, One,Left); (5,Blank, One) |-> (0, One,Left);
    (4,One, One) |-> (4,Blank,Left); (4,Blank, One) |-> (4,Blank,Left);
    (4,One, Blank) |-> (0,Blank,Right); (4,Blank, Blank) |-> (0,Blank,Right);]
    undefined;;

Now run the program with **exec** *prog* *inputlist* *oracle* *stage*:

    # exec prog [2] oracle 100;;
    result = 2
    use = 4- : unit = ()

If we view the oracle as the halting problem ***K***, then the above **prog** defines the function f(x):= if x belongs to ***K*** then x otherwise 0.

It is expected that input x should always be less than the stage s. This is not only for theoretical convenience but also the need of an output, since the machine should finish reading the whole input.

Since every oracle we can define here is computable (by writing down the algorithm explicitly), real-world o-machines seem to be no stronger than plain Turing machines. However, if there exists a truly random binary string, then this program may return you another one.
