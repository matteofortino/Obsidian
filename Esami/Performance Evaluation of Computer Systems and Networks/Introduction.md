#uni
# Performance Evaluation of Computer Systems and Networks 

# Recommended Books
## For students
S. M. Ross "Into the prob and statistic for engeneers and scientists"
## For further details
R. Jain "The art of comp systems performance and analysis"

# Introduction
Our goal in this course is to be able to determine if a system **A is better than B** under certain circumstances.
We have to define what *better* means; to be able to do that we need to have a performance metric we value to define the performace of the system which can be: 
- energy consuption
- throughput
- utilization
- response time
and so on...
There are also come characteristics that we should look up while deciding which system to choose which are:
- rest-life conditions
- comprehensivness
- look ahead
The most important thing remains that we need to be sure of how the data that we use to define our systems are calculated, for example if we are trying to mesure a response time of an http request from a server we can flood it and the either use the mean or the maximum of those requests. Let this be clear, **the maximum does not tell us nothing about the worst case**.
Now let's think about how we can measure those metrics, there are different ways we can do that:
- analytical set of equations
    - which are presented in the form of $o = f(i)$
    - this is not always possible because the systems can be time variant.
- software simulations
    - which will be covered ahead in this course
Those to technics still require us to be able to **manage randomness** so that we do not mistake random data as a structural property of the system.
# Probability Theory
What is probability? the best way to understand it is with **relative frequency** meaning that if you repeat an experiment **E** a number of times **N** while being on *indipendent conditions* which means that and outcome of the $i_{th}$ experiment does not influence any another the probability of a decided outcome $k$ is 
$$ 
P(E) = \lim_{N \to +\infty} \frac{k}{N}
$$
## Random experiment
Random expertinet are defined as experiment where we can not predict the outcome beforehand.
What we do it to define out expeced outcome based of course on the experiment for example: 
- **Experiment**: throwing a 6-faced dice, **outcome**: face on top.
After definig and outcome we can define a **sample space** which is the space which contains all the possible outcome of our experiment so $S = \{1,2,3,4,5,6\}$.
An **event** is a subset of the sample space $E = \{1,3,5\} \subseteq S$
## Set algebra
Since events are stes we can use **set algebra** to work with them, some of the rules are: 
- **Union**: $E \cup F$
- **Intersection**: $E \cap F$ which we will write as $EF$ 
- **Complement** $E^c$
Union and intersection follows the commutative and associative property as for the complement if follows the involutive one
there are also some fondamental laws called **De Morgans' Law's**:
- $(A \cup B)^c = A^c \cap B^c$
- $(A \cap B)^c = A^c \cup B^c$
## Axioms of probability
As we said a probability of and event is a number representing it relative frequency, now let's look at the axioms: 
1. $0 \leq P(E) \leq 1$
2. $P(S) = 1$
3. $E_i$ such that $E_i \cap E_j = E_i E_j =  \emptyset$  for every $i \neq j$ it means that $E_i$ and $E_j$ are **disjoint or mutally exclusive** $\Rightarrow$ $P(\bigcup_i E_i) = \sum_i P(E_i)$  
other important properties are:
- $P(E^c) = 1 - P(E)$
- $P(E_i \cup E_j)  = P(E_i) + P(E_j) - P(E_i E_j)$ 
# Characteristics of an Experiment 
Every experiment has its carasteristics which generally are: 
- $|S|$ is finite ($N$)
if all the outcomes are **equally likely** this means that $P = \frac{1}{N}$ and we can inferr that $P(E) = \frac{|E|}{|S|}$.
Also if we have and experiment $C$ which can be decomposed in smaller experiment $\{c_1, c_2, ..., c_k\}$ with finite cardinality $\{n_1,n_2,...,n_k\}$ the cardinality of $C$ is $\prod_{i = 1}^{k} n_i$. This is true if the combined experiment $C$ consists of performing all $k$ sub-experiments and an outcome is an ordered tuple of their outcomes.