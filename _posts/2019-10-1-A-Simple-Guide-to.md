In this four part series, we will talk about the following, 

Part 1: The need for algorithm analysis 

In the first part of this series, we are going to talk about the need for a structured approach to analyze algorithms. Suppose 
you and your friend, separately wrote algorithms for sorting a set of numbers. As computer scientists, our goal is to always find the 
most efficient algorithm for solving a problem. Suppose our goal was to minimize the time the algorithms took for sorting. How do we compare 
the algorithms? 

One way to do it is by giving the algorithms an input of a certain size, in this case a list of unsorted numbers and checking 
how long (in seconds) both algorithms take to sort the list. This could be one approach, but there are instances where one algorithm runs 
faster when the input size is small, but takes more time when the input size is large. What do we do to avoid this? 

If you and your friend are running your algorithms on two identical computers with the exact same specification, then the time 
taken could be a fair measure. But what if you both run it on two machines with different capabilities? What if your computer is 
10 years old with very low processing power, but your friend has a machine with greater hardware capabilities? 

Measuring the time would also not help in knowing how much faster one algorithm is over the other. It's hard to exactly quantify it for
 all instances. 
 
In order to solve the above issues, computer scientists over the years came up with a structured and more uniform way to analyze 
algorithms. One way to fix the problem of input sizes is making the run time as a function of the input size. Suppose $$ f $$ is 
the runtime of algorithm 1 and $$ g $$ is a function of algorithm 2, then $$ f(n) $$ would represent the runtime of the algorithm when 
the input is of size $$ n $$. For example, suppose we give our sorting algorithms a list of size 100, then $$ f(100) $$ and $$ g(100) $$ 
 measures the amount of time taken by the two algorithms for an input instance of size 100. 
 


Part 2: How to analyze the runtime of algorithms? 

Part 3: Growth functions
Part 4: Expressing the runtime of algorithms in terms of big-O etc
Part 5: Bounding the runtime of recurrences
