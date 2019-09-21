The Fibonacci numbers are one of the most well studied recurrence relations in history. It's also one of the coolest ways to get kids interested in math. The Fibonacci sequence is the following, 


$$ 
0,1,1,2,3,5,8,13,21,34,...
$$

Informally, the next term of the sequence is the sum of the last two terms in the sequence. More formally, suppose $$ f_n $$ is the nth Fibonacci numnber and $$ f_0 = 0 $$ and $$ f_1 = 1 $$, then 

$$ 
f_n = f_{n-1} + f_{n-2}
$$

There are a few ways to go about computing this number. Let us start easy and discuss the most intuitive way of going about computing this number. 

The naive way to computing the nth Fibonacci number is the following, 

$$
\begin{equation*}
\begin{split}
\text{fib}&(n): \\
& \text{if } n = 0,\text{ return } 0 \\
& \text{if } n = 1, \text{ return } 1 \\
& \text{return fib}(n-1) + \text{fib}(n-2)
\end{split}
\end{equation*}
$$ß

![Fibonacci series]({{ site.baseurl }}/images/IMG_0295.jpg){:class="img-responsive"}

The above algorithm does deliver the correct solution. However, the time complexity is $$ O(2^n) $$, which is quite terrible. The number of subproblems grows exponentially. 

In the above image, I've indicated the same subproblems computed by the same color. $$ f(n-3) $$ is represented in green and is computed thrice. At the second step, we need to compute 2 subproblems. At the third step, it becomes 8 subproblems. More generally, at the $$ i $$th step, we need to compute $$ 2^i $$ subproblems. The total time complexity is the sum of the work done (time complexity) at all levels. 

$$ 
T(n) = 1 + 2 + 4 + \ldots + 2^n = \sum_{i = 1}^n 2^i = 2^{n+1} -1 = O(2^n)
$$

A more clever implementation is the following which takes only $$ O(n) $$ time. 

$$ 
\begin{equation*}
\begin{split}
&A \leftarrow \text{array with } n+1 \text{ entries}\\
&A[0] = 0\\
&A[1] = 1 \\
&\text{for }i = 2\text{ to }n:\\
&\ \ \ \ A[i] \leftarrow A[i-1] + A[i-2]\\
&\text{return }A[n]
\end{split}
\end{equation*}
$$

The second algorithm trumps the first by not recomputing a Fibonacci number more than once. This is done by storing solutions to the smaller subproblems first and using it to compute solutions to bigger problems. This is an example of dynamic programming.

The next approach we will look at is how to compute the nth Fibonacci number using linear algebra. Specifically, we will look at the ideas of eigenvalues and eigenvectors for our problem and show how we can compute $$ f_n $$ in $$ O(\log n) $$. 

## Using Linear Algebra for Fibonacci

Let $$ f_i $$ be the $$ i^{\text{th}} $$ term of the sequence and let $$ f_0 = 0, f_1 = 1 $$. Let us consider a linear map $$ \mathcal{L} : \mathbb{N} \rightarrow \mathbb{N} $$ such that 

$$ 

\mathcal{L}(y,x) = (y + x, y) 

$$

Suppose we plug in $$ y = f_{n-1} $$ and $$ x = f_{n-2} $$, we get that 

$$ 

\mathcal{L}(f_{n-1},f_{n-2}) = (f_{n-1} + f_{n-2}, f_{n-1}) = (f_{n}, f_{n-1})

$$

We also get that 

$$ 

\mathcal{L}^2(f_{n-1},f_{n-2}) = \mathcal{L}(\mathcal{L}(f_{n-1},f_{n-2})) = \mathcal{L}(f_{n}, f_{n-1}) = (f_{n+1},f_n)

$$

We can show that 

$$ 

\mathcal{L}^{n-1}(f_1,f_0) = (f_n,f_{n-1}) 

$$

Every linear map induces a matrix representation and vice versa. Let $$ \mathcal{M} $$ be this matrix. From some calculations, one can find that 

$$ 

\mathcal{M} = 
\begin{bmatrix}
    1 & 1 \\
    1 & 0
\end{bmatrix} \\

\mathcal{M} \begin{bmatrix} f_{n-1} \\ f_{n-2} \end{bmatrix} = \begin{bmatrix}
    1 & 1 \\
    1 & 0
\end{bmatrix} \begin{bmatrix} f_{n-1} \\ f_{n-2} \end{bmatrix}= \begin{bmatrix} f_{n-1} + f_{n-2} \\ f_{n-1} \end{bmatrix} = \begin{bmatrix} f_n \\ f_{n-1} \end{bmatrix} \\

$$

Just like how $$ \mathcal{L}^{n-1}(f_1,f_0) = (f_n,f_{n-1}) $$, we get 

$$ 

\mathcal{M}^{n-1} \begin{bmatrix} f_{1} \\ f_{0} \end{bmatrix} = \begin{bmatrix}
    1 & 1 \\
    1 & 0
\end{bmatrix}^{n-1} \begin{bmatrix} f_{1} \\ f_{0} \end{bmatrix} = \begin{bmatrix} f_n \\ f_{n-1} \end{bmatrix}

$$

One way to go about it is compute $$ \mathcal{M}^{n-1} $$ in a clever way. Suppose $$ n - 1 $$ is a power of two, then we would first find $$ \mathcal{M}^2 $$, multiply it with itself upon which we get $$ \mathcal{M}^4 $$ and we repeat the process. We get the sequence of matrices, 

$$

\mathcal{M}, \mathcal{M}^2, \mathcal{M}^4, \mathcal{M}^8, \ldots, \mathcal{M}^{\frac{n-1}{2}}, \mathcal{M}^{n-1}

$$

Since our matrix $$ \mathcal{M} $$ is has dimension $$ 2 \times 2 $$, multiplying two matrices of that dimension costs $$ O(1) $$ operations. We would have to carry out $$ O(\log n) $$ multiplications that each take $$ O(1) $$ cost to compute $$ \mathcal{M}^{n-1} $$. Suppose $$ n-1 $$ is not a power of two, our algorithm can still be tweaked and it would still run in $$ O(\log n) $$.  

These is another way to arrive at a closed form formula for the Fibonacci number using the ideas of eigenvalues and eigenvectors. I'm going to give an extremely short introduction to the topic. I'll make a more detailed posts later on about eigenvalues and eigenvectors. 

Given a matrix $$ \mathcal{P} $$, a vector $$ v $$ is an eigenvector and $$ \lambda $$ is an eigenvalue corresponding to $$ v $$ if 

$$ 

\mathcal{P}v = \lambda v

$$

Since our matrix $$ \mathcal{M} $$ has full rank (rank is two), there are two non-zero eigenvalues $$ \lambda_1 $$ and $$ \lambda_2 $$. Let $$ (v_1, \lambda_1) $$ and $$ (v_2, \lambda_2) $$ be the corresponding eigenvector and eigenvalue pairs of $$ \mathcal{M} $$ such that $$ \mathcal{M}v_1 = \lambda_1v_1 $$ and $$ \mathcal{M}v_2 = \lambda_2v_2 $$. From some calculation, we can find that 

$$

\begin{equation*}
\begin{split}
v_1 & = \begin{bmatrix} \frac{1 + \sqrt{5}}{2} \\ 1 \end{bmatrix} \\
\lambda_1 & =  \frac{1 + \sqrt{5}}{2} \\
v_2 & = \begin{bmatrix} \frac{1 -\sqrt{5}}{2} \\ 1 \end{bmatrix} \\
\lambda_1 & =  \frac{1 -\sqrt{5}}{2} \\
\mathcal{Q} & = \begin{bmatrix} v_1 & v_2 \end{bmatrix} = \begin{bmatrix} \frac{1 + \sqrt{5}}{2} & \frac{1 -\sqrt{5}}{2} \\ 1 & 1 \end{bmatrix} \\
\mathcal{D} & = \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} = \begin{bmatrix} \frac{1 + \sqrt{5}}{2} & 0 \\ 0 & \frac{1 -\sqrt{5}}{2} \end{bmatrix}
\end{split}
\end{equation*}

$$

Suppose we do the following, 

$$ 

\begin{equation*}
\begin{split}
\mathcal{MQ} & = \mathcal{M}\begin{bmatrix} v_1 & v_2 \end{bmatrix} = \begin{bmatrix} \mathcal{M}v_1 & \mathcal{M}v_2 \end{bmatrix}\\
& = \begin{bmatrix}\lambda_1 v_1 & \lambda_2 v_2 \end{bmatrix} = \begin{bmatrix} v_1 & v_2 \end{bmatrix} \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix} \\
&= \mathcal{QD}
\end{split}
\end{equation*}
$$

The matrix $$ \mathcal{Q} $$ is full rank, hence its inverse must certainly exist. We will call this inverse $$ \mathcal{Q}^{-1} $$. The following is $$ \mathcal{Q}^{-1} $$. You can verify if $$ \mathcal{QQ}^{-1} $$ is the identity matrix. 

$$ 

\mathcal{Q}^{-1} = \begin{bmatrix} \frac{1}{\sqrt{5}} & \frac{\sqrt{5} -1}{2\sqrt{5}} \\ \frac{-1}{\sqrt{5}} & \frac{\sqrt{5}+1}{2\sqrt{5}} \end{bmatrix}

$$

We can finally show 

$$
\begin{equation*}
\begin{split}
\mathcal{MQ} & = \mathcal{QD} \implies \mathcal{M} = \mathcal{QDQ}^{-1} \\
\mathcal{M}^{n-1} & = \left(\mathcal{QDQ}^{-1}\right)^{n-1} = (\mathcal{QDQ}^{-1})(\mathcal{QDQ}^{-1}) \cdots (\mathcal{QDQ}^{-1}) \\
& = \mathcal{QD}(\mathcal{Q}^{-1}Q)D \cdots (\mathcal{Q}^{-1}\mathcal{Q})\mathcal{DQ}^{-1} \\ 
& = \mathcal{QD}^{n-1}\mathcal{Q}^{-1}
\end{split}
\end{equation*}

$$

We will also use another fact about diagonal matrices. Although we have used this fact for a diagonal matrix of size 2, using induction it is fairly straightforward to show it is true for a diagonal matrix of any dimension.

$$

\mathcal{D}^{n-1} = \begin{bmatrix} \lambda_1 & 0 \\ 0 & \lambda_2 \end{bmatrix}^{n-1} = \begin{bmatrix} \lambda_1^{n-1} & 0 \\ 0 & \lambda_2^{n-1} \end{bmatrix}

$$

Using the above fact, we have shown 

$$

\begin{equation*}
\begin{split}
\mathcal{M}^{n-1} & = \mathcal{QD}^{n-1}\mathcal{Q}^{-1} \\
& = \mathcal{Q}\begin{bmatrix} \lambda_1^{n-1} & 0 \\ 0 & \lambda_2^{n-1} \end{bmatrix}\mathcal{Q}^{-1}\\
& = \begin{bmatrix} \frac{1 + \sqrt{5}}{2} & \frac{1 -\sqrt{5}}{2} \\ 1 & 1 \end{bmatrix} 
\begin{bmatrix} \left(\frac{1 + \sqrt{5}}{2}\right)^{n-1} & 0 \\ 0 & \left(\frac{1 -\sqrt{5}}{2}\right)^{n-1} \end{bmatrix}
\begin{bmatrix} \frac{1}{\sqrt{5}} & \frac{\sqrt{5} -1}{2\sqrt{5}} \\ \frac{-1}{\sqrt{5}} & \frac{\sqrt{5}+1}{2\sqrt{5}} \end{bmatrix}\\
\mathcal{M}^{n-1} \begin{bmatrix} f_{1} \\ f_{0} \end{bmatrix} & = \begin{bmatrix} \frac{1 + \sqrt{5}}{2} & \frac{1 -\sqrt{5}}{2} \\ 1 & 1 \end{bmatrix} 
\begin{bmatrix} \left(\frac{1 + \sqrt{5}}{2}\right)^{n-1} & 0 \\ 0 & \left(\frac{1 -\sqrt{5}}{2}\right)^{n-1} \end{bmatrix}
\begin{bmatrix} \frac{1}{\sqrt{5}} & \frac{\sqrt{5} -1}{2\sqrt{5}} \\ \frac{-1}{\sqrt{5}} & \frac{\sqrt{5}+1}{2\sqrt{5}} \end{bmatrix} \begin{bmatrix} 1 \\ 0 \end{bmatrix} \\
& = \begin{bmatrix} \frac{1 + \sqrt{5}}{2} & \frac{1 -\sqrt{5}}{2} \\ 1 & 1 \end{bmatrix} 
\begin{bmatrix} \left(\frac{1 + \sqrt{5}}{2}\right)^{n-1} & 0 \\ 0 & \left(\frac{1 -\sqrt{5}}{2}\right)^{n-1} \end{bmatrix}
 \begin{bmatrix} \frac{1}{\sqrt{5}} \\ \frac{-1}{\sqrt{5}}  \end{bmatrix} \\ 
& = 
\begin{bmatrix} \left(\frac{1 + \sqrt{5}}{2}\right)^{n} & \left(\frac{1 -\sqrt{5}}{2}\right)^{n} \\ \left(\frac{1 + \sqrt{5}}{2}\right)^{n-1} & \left(\frac{1 -\sqrt{5}}{2}\right)^{n-1} \end{bmatrix}
 \begin{bmatrix} \frac{1}{\sqrt{5}} \\ \frac{-1}{\sqrt{5}}  \end{bmatrix} \\ 
 & = \begin{bmatrix} \frac{1}{\sqrt{5}}\left(\frac{1 + \sqrt{5}}{2}\right)^{n} - \frac{1}{\sqrt{5}}\left(\frac{1 - \sqrt{5}}{2}\right)^{n} \\ \frac{1}{\sqrt{5}}\left(\frac{1 + \sqrt{5}}{2}\right)^{n-1} - \frac{1}{\sqrt{5}}\left(\frac{1 - \sqrt{5}}{2}\right)^{n-1} \end{bmatrix} = \begin{bmatrix} f_{n} \\ f_{n-1} \end{bmatrix} 
\end{split}
\end{equation*}

$$

We have shown that 

$$ 
f_n = \frac{1}{\sqrt{5}}\left(\frac{1 + \sqrt{5}}{2}\right)^{n} - \frac{1}{\sqrt{5}}\left(\frac{1 - \sqrt{5}}{2}\right)^{n} 
$$ 

The value $$ \varphi = \frac{1 + \sqrt{5}}{2} $$ is called the golden ratio and tends to show up in many places. We can also write $$ f_n $$ as 

$$ 

f_n = \frac{\varphi^n - (1-\varphi)^n}{\sqrt{5}}

$$

The main bottle neck using this approach is computing the value of $$ \varphi^n $$ and $$ (1 - \varphi)^n $$ which can done in $$ O(\log n) $$ using the same doubling power technique we used to compute $$ \mathcal{M}^{n-1} $$.


**Warning**: We have made a few very strong assumptions here to say we can compute it in $$ O(\log n) $$. One of the core assumptions is computing the product of two numbers in $$ O(1) $$. As you can see with the closed form of Fibonacci which we derived, the numbers tend to have exponential growth. In reality, multiplying large numbers cannot be in $$ O(1) $$. For instance, we assumed that we could multiply $$ \varphi^{n/2} \times \varphi^{n/2} $$ to get $$ \varphi^n $$ in $$ O(1) $$ operations. Representing $$ \varphi^{n/2} $$ on a real computer would take $$ \Omega(n \log \varphi) $$ bits and multiplying it should take at least that, showing the cost of multiplication is $$ \omega(1) $$. 










