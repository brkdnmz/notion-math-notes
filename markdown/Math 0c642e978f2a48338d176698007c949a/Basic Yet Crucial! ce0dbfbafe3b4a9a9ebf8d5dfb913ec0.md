# Basic Yet Crucial!

## Let’s start with the most basic stuff.

![Russell Crowe as John Nash Jr. (A Beatiful Mind)](Basic%20Yet%20Crucial!%20ce0dbfbafe3b4a9a9ebf8d5dfb913ec0/Untitled.png)

Russell Crowe as John Nash Jr. (A Beatiful Mind)

## Integers

Integers are “whole” numbers. They can be either positive or negative:

$$
-31415, \dots, -3, -2, -1, 0, 1, 2, 3, \dots, 27182
$$

They are what we’ll be dealing with.

## Divisor

An integer $d$ is said to be a **divisor** of another integer $n$ if $n$ can be written as $n = k \cdot d$ where $k$ is also an integer.

In other words, we can reach $n$ by repeatedly adding $d$:

$$
d + d + d + \cdots + d = n
$$

Another approach can be that we can reach $0$ by repeatedly subtracting $d$ from $n$:

$$
n - d - d - d - \cdots - d = 0
$$

On the other way around, $n$ is a **multiple** of $d$. We say,

- $d$ divides $n$, or,
- $n$ is divisible by $d$

to highlight the relation.

Some examples are shown below:

| $\pm n$ | $d$ (negatives also count) |
| --- | --- |
| $1$ | $1$ |
| $2$ | $1, 2$ |
| $3$ | $1, 3$ |
| $10$ | $1, 2, 5, 10$ |
| $12$ | $1, 2, 3, 4, 6, 12$ |
| $16$ | $1, 2, 4, 8, 16$ |
| $17$ | $1, 17$ |
| $0$ | Every integer except $0$ |

For each divisor, its additive inverse (opposite signed) is also a divisor!

Basically, there are positive divisors and negative divisors.

You may check whether each divisor is valid or not by trying to find $k$ — it **must** be an integer!

## Divisibility

If $d$ is a divisor of $n$, we can express $n$ as $n = k \cdot d$.

But, what if $d$ is not a divisor of $n$?

For convenience, let’s assume both are positive.

If $d$ does not divide $n$, we **must not** be able to reach $0$ by repeated subtractions. So, there exist some consecutive steps as follows:

$$
\begin{aligned}
(s-1)^{th} \text{ step } &\to n - (s-1)d &> 0 \\
s^{th} \text{ step } &\to n - sd &< 0
\end{aligned}
$$

Let $r = n - (s-1)d$. We can combine the inequalities $r > 0$ and $r - d < 0$ as follows:

$$
0 < r < d
$$

Why do you think $r$ is named so?

It stands for the term $r$emainder!

A remainder can be thought of as the remained part left of $n$ in the last step of reaching $0$. It is “remained” because we cannot subtract $d$ from it anymore.

We can use the remainder to write an equation similar to $n = kd$:

$$
n = (s-1)d + r
$$

Did you realize that when $r = 0$, it looks similar to $n = kd$?

Voila!

> $d$ divides $n$ if and only if $r = 0$.
> 

It isn’t over yet. We don’t need the naive subtraction process to find $r$. Our computers have a powerful operation for that: the modulo operation! It uses the operator `%` to find $r$, i.e., `n % d == r`. 

The result is called *modulus*.

Below are some example modulo operations in `C++`:

```cpp
16  % 1 == 0
17  % 5 == 2
6   % 0 == error!
4   % 4 == 0
-17 % 5 == -2 (results in 3 in Python!)
4   % 145213421421 == 4
```

Using this operation, we can find the divisors of $n$ by iterating over $1 - n$ and checking whether the remainder is $0$:

```cpp
// https://ideone.com/k2WMUG
#include <iostream>
#include <vector>
using namespace std;
 
int main() {
	int n = 123456;
	vector<int> positive_divisors;
	for(int i = 1; i <= n; i++){
		if(n % i) continue; // r > 0
		// i is a divisor, do whatever you want!
		positive_divisors.push_back(i);
	}
	int n_positive_divisors = positive_divisors.size();
	cout << "n: " << n << "\n";
	cout << "Number of positive divisors: " << n_positive_divisors << "\n";
	cout << "Number of all divisors: " << 2 * n_positive_divisors << "\n";
	cout << "Positive divisors:\n";
	for(int d: positive_divisors){
		cout << d << " ";
	}
	cout << "\n";
}
```

```python
# https://ideone.com/0Cj8MK
n = 123456
positive_divisors = []
for i in range(1, n + 1):
    if n % i:  # r > 0
        continue
    # i is a divisor!
    positive_divisors.append(i)

n_positive_divisors = len(positive_divisors)
print(f"n: {n}")
print(f"Number of positive divisors: {n_positive_divisors}")
print(f"Number of all divisors: {2*n_positive_divisors}")
print("Positive divisors:")
print(" ".join(map(str, positive_divisors)))
```

## Primes

A prime number is a **positive** integer that has **exactly** $2$ different positive divisors (or $4$ divisors in total).

It is an integer’s purest form because a prime is not a “combination” (product) of smaller integers other than $1$.

Non-prime integers also have their own name: **composite**! Because they are **composed** of smaller integers greater than $1$.

Some integers are listed in the table below, and explained why they are prime/non-prime:

| $n$ | Prime or not? |
| --- | --- |
| $-2$ | None of the negative integers is considered prime. |
| $0$ | Divisible by every non-zero integer. Not prime. |
| $1$ | Has a single positive divisor rather than $2$. Not prime. |
| $2$ | Exactly two positive divisors: $1, 2$. Prime. |
| $3$ | Exactly two positive divisors: $1, 3$. Prime. |
| $4$ | Three positive divisors. Not prime. |
| $5$ | Exactly two positive divisors: $1, 5$. Prime. |
| $6$ | Four positive divisors. Not prime. |
| $7$ | Exactly two positive divisors: $1, 7$. Prime. |
| $8$ | Four positive divisors. Not prime. |
| $9$ | Three positive divisors. Not prime. |
| $10$ | Four positive divisors. Not prime. |
| $11$ | Exactly two positive divisors: $1, 2$. Prime. |
| $12$ | Six positive divisors. Not prime. |
| $13$ | Exactly two positive divisors: $1, 2$. Prime. |
| $14$ | Four positive divisors. Not prime. |
| $15$ | Four positive divisors. Not prime. |

### [Now that we have seen 'em all, let’s get to what we can do with this stuff!](Dive%20Deeper!%201341822373844f578ef5cec8ea189b00.md)