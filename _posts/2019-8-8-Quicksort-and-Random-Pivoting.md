Quicksort is the most widely used sorting algorithms in practice. It works extremely well both in terms of time and space complexity compared to its counterparts.
Given an unsorted list $$ x = [x_1, x_2, \ldots, x_n] $$, quicksort works by picking a pivot, creating two sublists, one with elements strictly less than the pivot and the other with elements strictly greater than the pivot. Quicksort is later recursively called on both sublists. Below is pseudocode for quicksort

$$ 
\begin{equation*}
\begin{split}
\text{function } &  \text{quicksort}(A):\\
&\text{if}\ A=\{x\}, \text{ return }x \\
&\text{Pick } p \in S \text{ as the pivot} \\
&\text{Partition}_1 = \text{ all elements less than } p \text{ in } S \\
&\text{Partition}_2 = \text{ all element greater than } p \text{ in } S \\
&\text{return } \{ quicksort(\text{Partition}_1), p, quicksort(\text{Partition}_2) \}
\end{split}
\end{equation*}
$$


Below is an animation I made illustrating the above algorithm.


{% include youtube.html id="vRjfrdJwpTE" %}

The base case is when there is a single element to be sorted. Choosing a pivot plays a pivotal role (pun intended) in the run time of the algorithm. The main cost associated with quicksort is the number of comparisons between the pivot and elements in the list. Ideally, we want to pick a pivot that is not too far from the median to prevent the depth of recursion (number of function calls) from being large and the number of comparisons being low. For example, in the best case, we pick the median to be the pivot and it splits the list into two equal halves. Suppose $$ T(n) $$ is the cost of quicksort, it would lead to, 

$$ T(n) = 2T\left(\frac{n}{2}\right) + O(n) $$

Solving the recurrence, we get $$ T(n)= O(n \log n) $$.


In the worst case, quicksort has runtime of $$ \Omega(n^2) $$. We will give an example of the worst case instance. Let $$ x = [x_1, x_2, \ldots, x_n] $$ be the list we wish to sort. 

$$y_1,y_2, \ldots, y_n = \text{quicksort}([x_1, x_2, \ldots, x_n]) $$

In the worst case, in the first step, the pivot $$ p = y_1 $$. In this case, the sublist $$ \text{Partition}_1 = \emptyset $$ and $$ \text{Partition}_2 = x - y_1 $$ or $$ [x_1, x_2, \ldots, x_n] - y_1 $$. In the first step, we are doing $$ n -1 $$ comparisons. When we run quicksort on $$ \text{Partition}_2 $$, in the worst case, the pivot is $$ y_2 $$ and the number of comparisons in $$ n - 2 $$. This trend continues where the size of the list decreases by 1 as the depth increases by 1. When the depth is $$ i $$, the number of comparisons is $$ n - i - 1 $$. 
In this worst case instance, the total number of comparisons is, 

$$ T(n) = \sum_{i = 1}^n n-i = \frac{n(n-1)}{2} = \Omega(n^2) $$

In randomized quicksort, instead of picking some arbitrary element to be the pivot, we pick the pivot uniformly at random. In the worst case, the run time of randomized quicksort is $$ \Omega(n^2) $$, but in the average case, or on expectation, it does extremely well and achieves $$ O(n \ln n) $$ runtime. We will now get into the analysis and prove that the expected runtime of randomized quicksort is $$ O(n \log n) $$. 


Let $$ Y_{ij} $$ where $$ i < j $$ be a Bernouilli or a 0-1 random variable where 

$$ 
\begin{equation*}
    Y_{ij}=
    \begin{cases} 
      1, & \text{if}\ y_i \text{ and } y_j \text{ are compared} \\
      0, & \text{otherwise}
    \end{cases}
  \end{equation*}
$$

In the worst case scenario, we mentioned before, all $$ Y_{ij} = 1 $$ where $$ i < j $$ 

$$ 
Y = \sum_{i<j} Y_{ij} = \frac{n(n-1)}{2} 
$$ 

But what were're interested in is, $$ \mathbb{E}[Y] $$ i.e., the expectated number of comparisons. We will use the fact that if $$ Y_1 $$ and $$ Y_2 $$ are random variables, then $$ \mathbb{E}[Y_1 + Y_2] = \mathbb{E}[Y_1] + \mathbb{E}[Y_2] $$. This property is called linearity of expectation. 

$$
\mathbb{E}[Y] = \mathbb{E}\left[\sum_{i<j} Y_{ij}\right] =  \mathbb{E}\left[\sum_{i=1}^{n-1} \sum_{j = i+1}^n Y_{ij}\right] = \sum_{i=1}^{n-1} \sum_{j = i+1}^n \mathbb{E}\left[Y_{ij}\right]
$$

Using linearity of expectation, all we need to do compute $$ \mathbb{E}[Y] $$ is computing the value of $$ \mathbb{E}[Y_{ij}] $$. Since $$ Y_{ij} $$ is a bernouilli random variable, 

$$ 

\begin{equation*}
\begin{split}
\mathbb{E}[Y_{ij}] &= 1\cdot \Pr[Y_{ij} = 1] + 0\cdot \Pr[Y_{ij} = 0] = \Pr[Y_{ij} = 1] \\
\mathbb{E}[Y] & = \mathbb{E}\left[\sum_{i<j} Y_{ij}\right] =  \mathbb{E}\left[\sum_{i=1}^n \sum_{j = i+1}^n Y_{ij}\right]\\
& = \sum_{i=1}^{n-1} \sum_{j = i+1}^n \mathbb{E}\left[Y_{ij}\right] = \sum_{i=1}^{n-1} \sum_{j = i+1}^n  \Pr[Y_{ij} = 1]
\end{split}
\end{equation*}
$$

What we are left with computing is the probability of two elements $$ y_i $$ and $$ y_j $$ being compared? Suppose we take a random ordering of numbers from 1 to 10 $$ [4,3,1,7,10,6,5,2,9,8] $$. Suppose we pick 5 to be the pivot. One sublist would be $$ [4,3,1,2] $$ and the other sublist would be $$ [7,10,6,9,8] $$. In this case, 5 is compared to every number in the original list, but no element in one sublist would be compared with a member of the other sublist. 

Suppose a pivot $$ p $$ is picked, there will be no future comparison between $$ y_i $$ and $$ y_j $$ where $$ y_i < p < y_j $$. We will make two important observations, 

1. Suppose $$ y_i $$ and $$ y_j $$ are compared, then either $$ y_i $$ or $$ y_j $$ is the pivot in that step. 
2. Suppose any elements in $$ \{ y_{i+1}, y_{i+2}, \ldots, y_{j-2},j_{j-1} \} $$ is chosen as the pivot, then $$ y_i $$ and $$ y_j $$ will never be compared. 

Hence, the only way $$ y_i $$ and $$ y_j $$ are compared is if $$ y_i $$ or $$ y_j $$ is picked as a pivot before any of the elements in $$ \{ y_{i+1}, y_{i+2}, \ldots, y_{j-2},j_{j-1} \} $$. This can be also rephrased as saying, what is the probability $$ y_i $$ or $$ y_j $$ is picked as the pivot first from the set $$ \{ y_i,y_{i+1}, \ldots,j_{j-1},y_j \} $$.

$$
\begin{equation*}
\begin{split}
\Pr[Y_{ij} = 1] & = \Pr[y_i \text{ or } y_j \text{ is picked as pivot first from } \{ y_i,y_{i+1}, \ldots,y_{j-1},y_j \}] \\
& = \Pr[y_i  \text{ is picked as pivot first from } \{ y_i,y_{i+1}, \ldots,j_{j-1},y_j \}] \\
&\ \ \ + \Pr[ y_j \text{ is picked as pivot first from } \{ y_i,y_{i+1}, \ldots,j_{j-1},y_j \}] \\
& = \frac{1}{j-i+1} +  \frac{1}{j-i+1}  \\
& =  \frac{2}{j-i+1} \\
\mathbb{E}[Y] & = \mathbb{E}\left[\sum_{i<j} Y_{ij}\right] =  \mathbb{E}\left[\sum_{i=1}^{n-1} \sum_{j = i+1}^n Y_{ij}\right]\\
& = \sum_{i=1}^{n-1} \sum_{j = i+1}^n \mathbb{E}\left[Y_{ij}\right] = \sum_{i=1}^{n-1} \sum_{j = i+1}^n  \Pr[Y_{ij} = 1] \\
& =  \sum_{i=1}^{n-1} \sum_{j = i+1}^n  \frac{2}{j-i+1} = \sum_{i = 1}^{n-1} \sum_{k = 1}^{n-i} \frac{2}{k+1} \\
& < \sum_{i = 1}^{n-1} \sum_{k = 1}^{n} \frac{2}{k} \\
& = \sum_{i = 1}^{n-1} O(\ln n) \\ 
& = O(n \ln n)
\end{split}
\end{equation*}
$$

We showed the expected runtime of randomized quicksort is $$ O(n \ln n) $$. It is quite interesting to analyze the probability of the runtime being far from the expected runtime. It can be shown randomized quicksort runs in $$ O(n \ln n) $$ with probability $$ 1 - \frac{1}{n} $$. I'll save this analysis for another post.

