## Introduction


{% include youtube.html id="o_rS8I52UxE" %}


Suppose I asked you how many people need to be in a room for two people to share the same birthday with 50% probability. What would your answer be? This is precisely what the birthday paradox is about. 

The paradox states the following, 
> If there are 23 people in the room, then with 50% probability, there will be two people sharing the same birthday.

Certain versions of the paradox has states something stronger, 
> If there are 70 people in the room, then with 99% probability, there will be two people sharing the same birthday. 

I found this to be quite surprising and unintuitive at first. Let us dig deeper into why this holds true. 
To simplify matters, we will make the following assumptions, 

1. We assume all people in the room are not born on a leap year. We are making this assumption to prevent having to analyze two different cases. 
2. There are no twins in the room. A pair of twins in the room would trivially result in two people having the same birthday. 
3. We assume people are born uniformly at random. What do we mean by that? People are likely to be born on any day of the year with equal probability. To formalize it a bit, pick any day of the year and the probability of being born on that day is $$ \frac{1}{365} $$.  
4. People are born independently of each other. This means the birth date of any person would have no affect on the birth date of another person. 

We must note that the above conditions don't necessarily hold in the real world. In particular, in the real world, people are not born uniformly at random. This [link](http://thedailyviz.com/2016/09/17/how-common-is-your-birthday-dailyviz/) has statistics on days people are born on. Although our model isn't an accurate depication of the real world, it's simplicity makes it significantly easier to analyze this problem. 

We must also note that if 366 or more people are present in a room, we are guaranteed two individuals will share the same birthday. This follows from the pigeonhole principle, if there are 366 people and 365 days, at least two people need to share the same birthday.

Suppose there was a single person in the room, then the probability that they share a birthday with someone else is 0. Let $$ (A_n) $$ be an event such that amongst $$ n $$ people, everyone shares a different birthday. Let $$ \Pr[A_n] $$ be the probability that everyone has a different birthday when there are $$ n $$ people in the room. Similarly, let $$ \overline{A_n} $$ be the complement of $$ A_n $$ i.e., the event where amongst $$ n $$ people, two people share the same birthday. 

$$
\Pr[A_n] + \Pr[\overline{A_n}] = 1
$$

$$ 
\Pr[A_1] = 1 \text{ because there is only one person in the room}
$$

Suppose there are two people in the room, person A and B. Without loss of generality, just to illustrate, suppose person A was born on January 1st. For person B and A to have different birthdays, person B must be born on any day except January 1st. There are 364 options for person B to be born so that their birthdays are on different days. 

$$
\Pr[A_2] = \Pr[\text{B is different from A}] = \frac{364}{365}
$$

Suppose there are three people, person A, B and C. For all of them to be born on different days, suppose person A was born on January 1st, then there are only 364 days for person B to be born from the previous example. Since person A and B take two days, person C can only be born on 365 - 2 = 363 days.

$$
\Pr[A_3] = \Pr[\text{B is different A and C is different from B,A}] = \frac{364}{365} \times \frac{363}{365}
$$

Something more interesting is happening here, suppose there are already $$ k - 1 $$ people in the room. When the $$ k ^{th} $$ person enters the room, for all $$ k $$ people to have a different birthday, the following two events need to be true

1. All the $$ k - 1 $$ people who entered the room before them need to have different birthdays. What is the probability of this? $$  \Pr[A_{k-1}] $$.
2. The $$ k^{th} $$ person needs to have a different birthday from all the other $$ k - 1 $$ people in the room. What is the probability of this happening? $$ \frac{365 - (k-1)}{365} $$.

We must also note that the two events above independent of each other since the $$ k^{th} $$ person is born independently of everyone elses birthday by assumption (4) stated at the start of this blogpost. 


$$ 
\begin{equation*}
\begin{split}
\Pr[A_k] & = \Pr [ k - 1 \text{ people in the room have different birthdays} \textbf{ AND } \\ 
& \ \ \ \ \ \ \ \text{person k has a different birthday from the } k - 1 \text{ people } ] \\
& =  \Pr [  k - 1 \text{ people in the room have different birthdays}] \\ 
& \ \ \ \ \times \Pr [\text{person k has a different birthday from the } k - 1 \text{ people } ] \\
& = \Pr[A_{k-1}] \times \frac{365 - (k-1)}{365}
\end{split}
\end{equation*}
$$

We will now compute probabilities, 

$$ 
\begin{equation*}
\begin{split}
\Pr[A_1] & = 1 \\
\Pr[A_2] & = \Pr[A_1] \times \frac{364}{365} = \frac{364}{365} \approx 0.997 \\
\Pr[A_3] & = \Pr[A_2] \times \frac{363}{365} = \frac{364}{365} \times \frac{363}{365} \approx 0.991 \\
\Pr[A_4] & = \Pr[A_3] \times \frac{362}{365} \approx 0.983 \\
& \vdots \\
\Pr[A_{22}] & = \Pr[A_{21}] \times \frac{344}{365} \approx 0.525 \\
\Pr[A_{23}] & = \Pr[A_{22}] \times \frac{343}{365} \approx 0.493 \\
\end{split}
\end{equation*}
$$

Since we have $$ \Pr[A_{23}] = 0.493 < 0.5 $$ , this would mean the probability that the probability two people share the same birthday is 

$$ 
\Pr[\overline{A_{23}} ] = 1 - \Pr[A_{23}]  = 1 - 0.493 = 0.507 > 0.5
$$

As $$ n $$ gets higher, the probability reaches 1. 

## Advanced


{% include youtube.html id="n-ar2YFPPck" %}


Suppose we want to go generalize this problem to the case where there are $$ n $$ people and $$ m $$ possible birthdays and given $$ n,m $$, can we define the probability with which two people share the same birthday? 

We can use the same setup as before, we will have the event $$ A_n $$ to signify all $$ n $$ people born on different days. In the case where there is only person, there is no change from before, 

$$
\Pr[A_1] = 1 
$$

In the case where there are two people, suppose there are two people, person A and B. For person B to have a different birthday, person B needs to have a birthday among the $$ m - 1 $$ other choices. 

$$
\Pr[A_2] = \frac{m-1}{m}
$$

In the case of three people, person B needs to have a birthday different from person A. Person C needs to have a birthday different from person A and B. 

$$
\Pr[A_3] = \frac{m-1}{m} \times \frac{m-2}{m}
$$

In general, for any $$ n $$, we can use the same recursive formula from the previous section, 

$$ 
\Pr[A_n] = \Pr[A_{n-1}] \times \frac{m - (n-1)}{m}
$$

Suppose we want to find a closed form expression for $$ \Pr[A_n] $$, we will expand the expression, 

$$
\begin{equation*}
\begin{split}
\Pr[A_n] & = \Pr[A_{n-1}] \times \frac{m - (n-1)}{m} \\
& = \Pr[A_{n-2}] \times \frac{m - (n-2)}{m} \times \frac{m - (n-1)}{m} \\
& = \Pr[A_{n-3}] \times \frac{m - (n-3)}{m} \times \frac{m - (n-2)}{m} \times \frac{m - (n-1)}{m} \\
& \ \vdots \\
& = \Pr[A_2] \times \frac{m-2}{m} \times \frac{m-3}{m} \times \cdots \times \frac{m - (n-3)}{m} \times  \frac{m - (n-2)}{m} \times \frac{m - (n-1)}{m} \\
& = \prod_{i = 1}^{n-1} \frac{m-i}{m} = \prod_{i = 1}^{n-1} 1 - \frac{i}{m} \\
& \approx \prod_{i = 1}^{n-1} e^{\frac{-i}{m}} \text{ using the identity } 1 - x \approx e^{-x} \\
& = e^{-\sum_{i = 1}^{n-1} \frac{i}{m}} \\
& = e^{\frac{-n(n-1)}{2m}} \approx e^{\frac{-n^2}{2m}}
\end{split}
\end{equation*}
$$

An approximation for this result is $$ \Pr[A_n] \approx e^{\frac{-n^2}{2m}} $$. While it is possible to find tighter bounds, this gives us a reasonable approximation. The only identity we used for this approximation is the fact that 

$$
1 - x \approx e^{-x}
$$

The birthday paradox abstractly has numerous applications in computer science. One area in particular is hashing, which has numerous uses of its own. Ideas in this problem are key to analyzing the probability with which two elements hash to the same key. This problem can be further generalized to the problem in probability known as the balls and bins problem, which we will save for another blog post. To conclude this post, we leave the following questions which might be of interest to the reader, 

## Applications 


{% include youtube.html id="wFTFE2Ij3o4" %}


The birthday paradox isn't a just a toy problem or a neat party trick, it has numerous real world applications. I personally find the probabilistic analyis used in the proofs to be helpful when analyzing other problems involving randomization. One particular application is in the area of designing cryptographic hash functions. In particular, we will look at how the analysis from above can be quite useful in designing hash functions against birthday attacks, which we will study in more depth. 

First, let us define what a hash function is. A hash function $$ h : U \rightarrow [0,m-1] $$ is a function which maps from a set $$ U $$ to a number in $$ [0,m-1] $$. A function $$ h $$ defined as $$ h(x) = x \mod m $$ is an example of a hash function from $$ \mathbb{N} \rightarrow [0,m-1] $$. Hash functions have numerous uses of its own, especially in conjunction with hash tables, a popular data structure. It also has uses in cryptography where one uses a specific kind of hash function called cryptographic hash function. 

One of the many properties that cryptographic hash functions need to have is collision resistance. It must be difficult to find two $$ u_1, u_2 \in U $$ such that they collide i.e., $$ h(u_1) = h(u_2) $$. 

Collision resistance is what we will turn our focus to. First, we will explain why collision resistance is a desired property. In order to show that, we will first explore digital signatures. 

In the past, people and organization used signatures and authorization stamps to signify they have "signed" a document. Recently, there has been a push to do this electronically or digitally. A digital signature needs to satisfy three main properties. 

1. When a document is signed, one should be able to verify who signed the document. 
2. After a document has been signed, no one should be able to tamper with the document. 
3. A person who signed a document cannot later deny signing the document. 

A digital signature is a way of provably verifying the authenticity of a document or a message. A digital signature would guarantee that the message received was created by a known sender and was not altered. 

Suppose we have a document $$ m $$, how do we sign the document? 

Each user/organization has a unique private key $$ p_k $$ and a public key $$ pub_k $$ associated with them. They use a signing function to sign the document with their private key. However, a digital signature function can only sign a small number of documents. The domain of the signing function is small. One way to solve this problem is by creating a smaller document which represents the original document, but has a much smaller size. Most often, this is done by using a hash function on the larger document. A hash function is used to map it from a bigger space to a smaller space and the result is called the digest. The signature would use the digest and private key to create a signature. The procedure can be described as follows: 

1. Obtain private key $$ p_k $$. 
2. Hash the document $$ m $$ and obtain $$ h(m) $$. 
3. Sign $$ h(m) $$ using the private key $$ p_k $$. 
4. Send $$ sign(h(m), p_k) $$ and the public key $$ pub_k $$ to anyone who wishes to verify the document.

Anyone who looks at $$ sign(h(m), p_k)) $$ and the public key will be able to verify if the person indeed signed document $$ m $$. 

Problem: Suppose an evil adversary, Eve, finds two documents $$ m, m' $$ where $$ m $$ is a fair contract and $$ m' $$ is a fraudulent document such that $$ h(m) = h(m') $$. Suppose Eve knows ahead of time that Alice would only sign $$ m $$ and not $$ m' $$. A cryptographic hash function $$ h $$ is applied on the document before it is signed. Eve goes to Alice and asks her to sign the document $$ m $$ using the above steps. Now, Eve can claim Alice signed the document fraudulent $$ m' $$ since $$ h(m) = h(m') $$. There is no way Alice can prove she did not sign $$ m' $$. 

In the above example, since $$ h $$ was not a collision resistant function, Eve was able to find two inputs which hashed to the same value. Eve was able to claim Alice signed $$ m' $$ when in fact, she only signed $$ m $$. This highlights the importance of collision resistance and shows why digital signatures are susceptible when the hash function used is not collision resistant. 

Let $$ h : U \rightarrow [0,m-1] $$ be a hash function. Suppose assume the input is hashed to any of the $$ m $$ elements uniformly and independently at random. 

The motivation for the birthday attack is to find the smallest number of elements, $$ n $$ to be hashed using $$ h $$ so that we find two elements $$ x, y \in U $$ such that $$ h(x) = h(y) $$. 

In a birthday attack, an adversary would pick $$ x \in U $$ at random and store the pairs $$ (x, h(x)) $$. The adversary would pick repeatedly pick and store these pairs until they find two values $$ x, y $$ such that $$ h(x) = h(y) $$. We want to know how many times the attacker needs to repeat this until they find a collision. 

We can use the exact same ideas from the birthday paradox to analyze the birthday attack. In the birthday attack, $$ m $$ happens to be the number of days in a year and $$ U $$ is analogous to people entering the room. People are hashed to their birthdays, which could be one of $$ m $$ values. Suppose we want to find a collision with probability 99%. We want to know what is the smallest $$ n $$ such that two values hash to same birthday (or in hash function world, two inputs hash to the same value.

We previously showed $$ \Pr[A_n] \approx e^{\frac{-n^2}{2m}} $$

We want to set $$ \Pr[\overline{A}_n] \approx e^{\frac{-n^2}{2m}} = \frac{99}{100} $$, so we want to set $$ \Pr[A_n] \approx e^{\frac{-n^2}{2m}} = \frac{1}{100} $$

$$ 

\begin{equation*}
\begin{split}

e^{\frac{-n^2}{2m}} & = \frac{1}{100} \\
\frac{n^2}{2m} & = \ln 100 \\
 n^2 & = 2 m \ln 100 \\
n & = \sqrt{2 m\ln 100 }

\end{split}
\end{equation*}

$$

The above analysis tells us that in order to have a collision in a hash function with range of size $$ m $$, one hash to uniformly and independently hash approximately $$  \sqrt{2 m\ln 100 } = O(\sqrt{m}) $$ to almost guarantee (99% probability) that two items hash to the same value. 

Suppose we had wanted to have a collision with 50% probability, then we would need $$ n = \sqrt{2 m \ln 2 } $$. The important takeaway is that we would in the order of $$ \sqrt{m} $$ elements to be hashed in order to get a collision with probability more than a half. This is consistent with our previous analysis of $$ m = 365 $$ since $$ \sqrt{2 \ln 2 \cdot 365} $$ is roughly 23. 



# Additional Problems 

1. Given $$ n $$ people, $$ m $$ days and some number $$ k $$, find the probability exactly $$ k $$ people share the same birthday. 
2. Let us modify the above problem a little bit. Suppose we were given $$ m $$ days and some number $$ k $$. What is the smallest value of $$ n $$ such that there are at least $$ k $$ people who share the same birthday with probability at least 0.5? Can you generalize it for any probability $$ p > 0 $$? 
2. Suppose you have 100 numbers from 1 to 100 and also a machine that guesses a random number between 1 and 100 uniformly at random. How many times would you need to use the machine on expectation so that the machine guesses all numbers between 1 and 100? 
3. Can you generalize the above problem for any $$ n $$?
3. Suppose we have a hash function which assigns elements to buckets uniformly at random. Suppose there $$ m $$ buckets. How many elements $$ n $$ need to be added to our data structure on expectation so that every bucket has at least two elements hashed to it? 


