# GCD & LCM: Powerful Twins!

## Until this point, we’ve seen:

- **Divisors**
- **Divisibility & remainder**
- **Primes & prime factorization**
- **Sieve of Eratosthenes**

## These are more than enough to understand what GCD and LCM are, and how to find them.

![Spider-Mans pointing at each other](https://media.tenor.com/QXVs4QWLlzkAAAAC/spider-man.gif)

Spider-Mans pointing at each other

## GCD, a.k.a. Greatest Common Divisor

To define GCD, we need a group $I$ of $n$ integers $i_1, i_2, \dots, i_n$ such that at least one of them is non-zero. ($n$ is often $2$.)

Ignore the “greatest” adjective, and focus on “common divisor”.

> A common divisor of $I$ is an integer $c$ that divides each integer in $I$.
> 

So, for $G = \{2, 4, 6\}$, there are $4$ common divisors: $\pm 1, \pm 2$.

But is there any reasonable method for finding a common divisor?

Of course! Remember that for an integer $d$ to divide $n$, each prime’s exponent in $d$’s prime factorization should be less than or equal to that in $n$’s prime factorization.

Therefore, if we factorize integers in $I$:

$$
\begin{aligned}
i_1 &= p_1^{e_{1, 1}} \cdot p_2^{e_{1, 2}} \cdot p_3^{e_{1, 3}} \cdots \\
i_2 &= p_1^{e_{2, 1}} \cdot p_2^{e_{2, 2}} \cdot p_3^{e_{2, 3}} \cdots \\
i_3 &= p_1^{e_{3, 1}} \cdot p_2^{e_{3, 2}} \cdot p_3^{e_{3, 3}} \cdots \\
&\vdots \\
i_n &= p_1^{e_{n, 1}} \cdot p_2^{e_{n, 2}} \cdot p_3^{e_{n, 3}} \cdots
\end{aligned}
$$

> **Here, assume $0$’s factorization has each exponent equal to $\infty$ because $0$ is divisible by every non-zero integer.**
> 

And also factorize $c$:

$$
c = p_1^{f_1} \cdot p_2^{f_2} \cdot p_3^{f_3} \cdots
$$

We can quickly infer that for each prime $p_k$, it should hold that:

$$
\begin{aligned}
&f_k \le e_{1, k} \\
&f_k \le e_{2, k} \\
&f_k \le e_{3, k} \\
&\vdots
\end{aligned}
$$

In English, each exponent in $c$’s factorization should be less than or equal to the same prime’s exponent in each integer’s factorization in $I$. This guarantees $c$ to divide every integer in $I$.

We can combine the inequalities above into one by using the $\min$ function.

> $m = \min(a_1, a_2, a_3, \dots, a_n)$ is the minimum of those $n$ integers. Formally, $m$ is the largest possible integer satisfying $m \le a_i$ for each $i$.
> 
> 
> The expression can be shortened as $m = \min\{a_i\}$.
> 

Thus, we can write $0 \le f_k \le \min\{e_{j, k}\}$.

To find a proper $c$, it is enough to choose an exponent $f_k$ for each prime $p_k$.

Now, put “greatest” back. How to find the **greatest** common divisor?

The answer is trivial now: choose the maximum possible $f_k$. Formally,

$$
\Large \text{GCD}(I) = \text{GCD}(i_1, i_2, \dots, i_n) = \prod_k p_k^{\min\{e_{j, k}\}}
$$

> That is, for each prime, choose the minimum exponent in factorizations of integers in $I$ — it’s what the exponent in GCD’s factorization is.
> 

> **Note that you may ignore $0$ since it cannot ever contain the minimum exponent for any prime.
However, if $I$ only contains $0$’s, then GCD is considered undefined.**
> 

Here are some example GCD calculations:

| $I$ | $\text{GCD}$ |
| --- | --- |
| $2, 4, 6$ | $2 = 2^1, 4 = 2^2, 6 = 2 \cdot 3$.
$\text{GCD} = 2^{\min\{1, 2, 1\}} \cdot 3^{\min\{0, 0, 1\}} = 2$.
The other primes may be ignored since their exponents are zero in all integers. |
| $28, 42$ | $28 = 2^2 \cdot 7, 42 = 2 \cdot 3 \cdot 7$.
$\text{GCD} = 2^{\min\{2, 1\}} \cdot 3^{\min\{0, 1\}} \cdot 7^{\min\{1, 1\}} = 14$. |
| $0, 21421421$ | GCD of any two integers where exactly one of them is zero is equal to the non-zero integer. It’s $21421421$ in this case. |
| $1, 2, 3, 4, 5$ | In $1$’s prime factorization, every exponent equals $0$. So, GCD is trivially $1$. |
| $314, 314$ | The integers are identical, and so are the exponents in factorization. GCD is $314$. |

### GCD can be calculated via iterations.

Inspect the $\min$ function. Suppose we pass it $n$ integers $a_1, a_2, \dots, a_n$, i.e., we are trying to find $\min\{a_1, a_2, \dots, a_n\}$.
Notice that the answer (the minimum integer) does not change if we first split these $n$ integers into $g$ subgroups, then calculate $\min$ of each subgroup, and finally calculate $\min$ of these $g$ $\min$s. It’s because at least one of them will be equal to the overall minimum integer, and we’ll obtain it after the final $\min$ calculation.
For instance, let’s say $n = 6$ and integers are $10, 4, 5, 16, 3, 8$ ($\min = 3$), and split them into $3$ groups: $\{10\}, \{4, 5, 16\}, \{3, 8\}$. Then, the $\min$s of these groups are $10, 4, 3$, respectively. Finally, $\min\{10, 4, 3\} = 3$. See? It’s the overall $\min$.

The opposite of this also holds, that is, two subgroups can be merged into one. $\min$ is not going to change.

How does this help?

GCD is pretty much all about calculating the $\min$s of exponents in factorizations. So, we can calculate the GCD of $I$ by iterating over the integers one by one. Let’s take an exemplary $I$ and demonstrate this process.

1. $I = \{36, 18, 12, 16, 24\}$
2. $\text{GCD}(I) = \text{GCD}(36, 18, 12, 16, 24) = \text{GCD}(\text{GCD}(36, 18), \text{GCD}(12, 16, 24))$
3. $\text{GCD}(\text{GCD}(36, 18), \text{GCD}(12, 16, 24)) = \text{GCD}(18, \text{GCD}(12, 16, 24)) = \text{GCD}(18, 12,\text{GCD}(16, 24)) = \text{GCD}(\text{GCD}(18, 12),\text{GCD}(16, 24))$
4.  
$\text{GCD}(\text{GCD}(18, 12),\text{GCD}(16, 24)) = \text{GCD}(6, 8) = 2$

Hence, $\text{GCD}(I) = 2$.

## GCD w/o Prime Factorization (Euclid’s Algorithm)

In my opinion, GCD is best understood via prime factorization due to selecting minimum exponents. However, some observations become much clearer when we ignore factorization.

Remember the “divisor expression”? It was $n = dk$ where $d$ is a divisor of $n$. We can use it for GCD too!

From now on, let’s only deal with two integers — it will make our lives better.

Take two integers $a, b$, and let $\text{GCD}(a, b) = g$. Here are the observations we can make at first glance:

- Since $g$ is their common divisor, we can express them as $a = g \cdot x$ and $b = g \cdot y$.
- Since $g$ is their **greatest** common divisor, then $\text{GCD}(x, y) = 1$. **(This type of pairs are called coprime, because they don’t share a common divisor other than $1$.)**
    
    > Otherwise, if $\text{GCD}(x, y) = g' > 1$, we could also write $x = g' \cdot x'$ and $y = g' \cdot y'$. Then, it would be the case that $a = g \cdot g' \cdot x'$ and $b = g \cdot g' \cdot y'$, then $GCD(a, b) \ge g \cdot g' > g$. But we started by saying that GCD is equal to $g$.
    > 

Here is the question:

> Does GCD change at all when we subtract one number from another, e.g., subtract $b$ from $a$?
> 

Let’s find out. $a-b = g(x-y)$ and $b = g \cdot y$. Both are divisible by $g$, so their GCD is divisible by $g$ (at least $g$).

Let $\text{GCD}(x-y, y) = g'$. Then, their sum $x$ is also divisible by $g'$. Thus, $\text{GCD}(x, y)$ is at least $g'$ because $g'$ is a common divisor of $x$ and $y$.

But we know that $\text{GCD}(x, y) = 1$. As a consequence, $g'$ must be $1$. So, $\text{GCD}(x-y, y) = 1$, and as they don’t share a common divisor, $\text{GCD}(a-b, b) = g$.

> The answer is, GCD **does not change** after subtraction. In fact, it also holds for addition.
> 

How does this help?

Recall the **modulo** operation. It is used to find the remainder, and the remainder is basically the result of repeated subtractions $a - b - b - b \dots$

If GCD does not change after a single subtraction, then it does not also change after multiple subtractions. This lets us use the modulo operation to calculate GCD:

$$
\text{GCD}(a \ \% \ b, b) = GCD(a, b)
$$

And, this is how GCD can be calculated so fast. We repeatedly apply the modulo operation on the bigger side ($a$ if $a > b$, otherwise $b$) until one becomes zero. In that case, GCD is equal to the other.

Here are some examples:

| $a$ | $b$ | $\text{GCD}$ |
| --- | --- | --- |
| $36$ | $42$ | $\text{GCD}(36, 42) = \text{GCD}(36, 6) = \text{GCD}(0, 6) = 6$ |
| $0$ | $5$ | One is already zero. GCD is $5$. |
| $55$ | $34$ | $\text{GCD}(55, 34) = \text{GCD}(21, 34) = \text{GCD}(21, 13) = \text{GCD}(8, 13) = \text{GCD}(8, 13) = \text{GCD}(8, 13) = \text{GCD}(8, 5) = \text{GCD}(3, 5) = \text{GCD}(3, 2) = \text{GCD}(1, 2) = \text{GCD}(1, 0) = 1$

Wow, so many steps! |
| $10000$ | $1$ | $\text{GCD}(10000, 1) = \text{GCD}(0, 1) = 1$ |

Why did the third example take so many steps? It’s because the worst case occurs when $a$ and $b$ are consecutive Fibonacci numbers! (Figure out why)

Because of this, the time complexity of GCD calculation is $\mathcal{O}(\log \min(a, b))$.

> This method is also known as the “Euclidean Algorithm”, which is referred to on [Wikipedia](https://en.wikipedia.org/wiki/Greatest_common_divisor).
> 

## Implementation

```cpp
// https://ideone.com/MZLH0V
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

ll gcd(ll a, ll b) {
    // always assign a = a % b for convenience
    if (a < b)
        swap(a, b); // built-in function

    while (b) { // while b is non-zero, continue the process
        a %= b;
        swap(a, b);
    }
    // at the end, a will be the GCD!
    return a;
}

ll gcd_recursive(ll a, ll b) {
    if (a < b)
        swap(a, b);
    if (!b)
        return a;
    return gcd_recursive(a % b, b);
}

// You don't need to swap yourself. This is an alternative version.
ll gcd_recursive_alt(ll a, ll b) {
    if (!a) // a will eventually become zero.
        return b;
    // First, change the parameters' location,
    // then apply modulo. This is totally the same.
    return gcd_recursive_alt(b % a, a);
}

int main() {
    ll a = 152348643862835682ll;
    ll b = 4236438563286243ll;
    cout << "GCD (iterative):\t" << gcd(a, b) << "\n";
    cout << "GCD (recursive):\t" << gcd_recursive(a, b) << "\n";
    cout << "GCD (recursive v2):\t" << gcd_recursive_alt(a, b) << "\n";

    // Guess what? There is a  built-in GCD function!
    cout << "GCD (built-in):\t\t" << __gcd(a, b) << "\n";
}
```

```python
# https://ideone.com/U43wXa
from math import gcd

def gcd_iterative(a: int, b: int) -> int:
    if a < b:
        a, b = b, a

    while b:
        a %= b
        a, b = b, a

    return a

def gcd_recursive(a: int, b: int) -> int:
    if a < b:
        a, b = b, a

    if not b:
        return a

    return gcd_recursive(a%b, b)

def gcd_recursive_alt(a: int, b: int) -> int:
    if not a:
        return b

    return gcd_recursive_alt(b%a, a)

a = 152348643862835682
b = 4236438563286243

print(f"GCD (iterative):\t{gcd_iterative(a, b)}")
print(f"GCD (recursive):\t{gcd_recursive(a, b)}")
print(f"GCD (recursive v2):\t{gcd_recursive_alt(a, b)}")
print(f"GCD (built-in):\t\t{gcd(a, b)}")
```

### Output

```
GCD (iterative):	  3
GCD (recursive):	  3
GCD (recursive v2):	3
GCD (built-in):		  3
```

### Problems to practice

- Prove that $\text{GCD}(ka, kb) = k \cdot \text{GCD}(a, b)$.
- Prove that $\text{GCD}(a+b, b) = \text{GCD}(a, b)$.
- For which integer values of $n$ under $200$ is $\frac{28n + 46}{5n + 13}$ reducible?
- If we know that the GCD of a group of integers is $360$, then how many different common divisors do these integers share?
- If $x$ and $y$ are coprime, prove that $\frac{x^2 + y^2}{xy}$ is irreducible.
- Find all positive integers $n$ such that each of $373, 493, 733, 673$ produces a remainder of $13$ when divided by $n$.

## LCM, a.ka. Least Common Multiple: GCD’s Elder Brother

As its name describes, LCM is the **least** of **positive** common multiples. Remember what we did with prime factorizations to find GCD: we chose the **minimum** exponent for each prime.

For LCM, it’s very similar: we choose the **maximum** exponent for each prime.

Because it gives us the smallest common multiple possible among infinitely many common multiples.

There isn’t really much else to explain. 😃 It can be defined formally similar to how we defined GCD:

$$
\Large \text{LCM}(I) = \text{LCM}(i_1, i_2, \dots, i_n) = \prod_k p_k^{\max\{e_{j, k}\}}
$$

> If a group of integers contains $0$, unlike GCD, LCM cannot be computed since $0$ does not have a multiple other than itself. This isn’t a logical scenario anyway.
> 

Here are some example LCM calculations:

| $I$ | $\text{LCM}$ |
| --- | --- |
| $2, 4, 6$ | $2 = 2^1, 4 = 2^2, 6 = 2 \cdot 3$.
$\text{LCM} = 2^{\max\{1, 2, 1\}} \cdot 3^{\max\{0, 0, 1\}} = 12$.
The other primes may be ignored since their exponents are zero in all integers. |
| $28, 42$ | $28 = 2^2 \cdot 7, 42 = 2 \cdot 3 \cdot 7$.
$\text{LCM} = 2^{\max\{2, 1\}} \cdot 3^{\max\{0, 1\}} \cdot 7^{\max\{1, 1\}} = 84$. |
| $1, 21421421$ | LCM of any two integers where at least one of them is $1$ is equal to the other integer. It’s $21421421$ in this case. |
| $1, 2, 3, 4, 5$ | $1 = 1, 2 = 2, 3 = 3, 4 = 2^2, 5 = 5$.
$\text{LCM} = 2^2 \cdot 3 \cdot 5 = 60$. |
| $314, 314$ | The integers are identical, and so are the exponents in factorization. LCM is $314$. |

LCM has the same “splittable” property, similar to GCD. We can calculate LCM in any way we want.

## A special case: GCD and LCM of $2$ integers

Take any two integers $a$ and $b$. Notice that for GCD, we take the smaller exponent and for LCM, we take the bigger, thus, the other exponent from prime factorizations.

Once we wrote that down, we can observe that the sum of exponents in GCD and LCM is equal to the sum of exponents in $a$ and $b$.

What do the sums of exponents represent?

**Product!**

Hence the equality:

$$
a \cdot b = \text{GCD}(a, b) \cdot \text{LCM}(a, b)
$$

Note that the only case that this definitely holds is **there are exactly two integers**. It might also hold when there are more integers, but it’s not guaranteed.

With this equality, we can calculate LCM in logarithmic complexity, thanks to GCD:

$$
\text{LCM}(a, b) = \frac{a \cdot b}{\text{GCD}(a, b)}
$$

### **I suggest you always think about prime factorization when dealing with these.**

### [This was the last topic about divisibility (kind of). It’s time to learn how to count!](Counting!%20454e72883095454d844770db6ff187b4.md)