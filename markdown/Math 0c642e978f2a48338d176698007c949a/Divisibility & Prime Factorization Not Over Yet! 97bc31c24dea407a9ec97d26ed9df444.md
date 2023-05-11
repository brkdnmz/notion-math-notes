# Divisibility & Prime Factorization: Not Over Yet!

## Math is cooool enough to provide prime factorization. We are ready to explore further.

![Excited Homelander (The Boys)](https://i.kym-cdn.com/photos/images/newsfeed/002/403/577/df9.gif)

Excited Homelander (The Boys)

Let’s take a general integer $n$ and write down its prime factorization. We’ll use it throughout the whole page:

$$
\huge \color{blue}{n} = \color{purple}{p_1^{e_1}} \cdot \color{orange}{p_2^{e_2}} \cdot \color{darkcyan}{p_3^{e_3}} \cdot \color{red}{p_4^{e_4}} \cdots
$$

Also, $d(n)$ stands for the number of positive divisors of $n$.

## Number of Divisors

Remember how we iterated up to $\sqrt{n}$ to find the number of divisors of $n$?

We can count them by just $n$’s prime factorization. There are a few properties to note down:

- If a divisor $d$ is divisible by a prime $p$, so is $n$. That’s because $d = mp$ and $n = kd$, thus, $n = kmp$.
- If a divisor $d$ is divisible by a **prime power** $p^e$, so is $n$.

These imply the following:

> **The largest power of a prime $p_i$ dividing $d$ is $p_i^{e_i}$. The least is trivially $p_i^0 = 1$.**
> 

So, it boils down to choosing an exponent $e_i'$ within $[0, e_i]$ for each $p_i$ to construct $d$. Formally,

$$
\huge d = p_1^{e_1'} \cdot p_2^{e_2'} \cdot p_3^{e_3'} \cdot p_4^{e_4'} \cdots
$$

Therefore, there are $\prod (e_i+1)$ different positive divisors of $n$. The total number of divisors is two times that.

### Some derived problems to exercise

- The number of even divisors
- The number of odd divisors
- The number of perfect square divisors
- The number of divisors that can be written as some integer to the power $k$, i.e, $x^k$ ($k$ is a non-negative integer)
- The smallest value of the largest exponent in the prime factorization for an integer with $k$ positive divisors
- The number of integers with $k$ positive divisors that are divisible by first $m$ smallest primes, where $m$ has the maximum possible value

## More on Complementary Divisors

### Sometimes, it’s just OK to ignore prime factorization, because there is this other perspective.

Let’s recall the complementary divisors:

> Two divisors $d$ and $k$ of $n$ complement each other if their product is equal to $n$.
> 

Formally,

$$
\large d \cdot k = n
$$

As you remember, one is $\le \sqrt{n}$ and the other is $\ge \sqrt{n}$. WLOG, let’s assume $d \le \sqrt{n}$ and $k \ge \sqrt{n}$.

For each $d$, $k$ is unique since $k$ decreases when $d$ increases. Therefore, there are $\left \lfloor \frac{d(n)+1}{2} \right \rfloor$ such complementary pairs. Here, the “floor division” notation is used. It gives the integer part of the division, i.e., ignores (subtracts) the remainder before division. The formula is like so because of the case where $n$ is a perfect square. In that case, every $2$ divisors form a pair, and then $\sqrt{n}$ forms the last pair itself, where $d = k = \sqrt{n}$.

By thinking of complementary divisors, we can solve some problems easier than we could with prime factorization. For instance, ignore the assumption that $d \le k$ and write down the product for each pair in the order of increasing $d_i$:

$$
\begin{aligned}
d_1 \cdot k_1 &= n \\
d_2 \cdot k_2 &= n \\
d_3 \cdot k_3 &= n \\
&\vdots \\
d_{d(n)} \cdot k_{d(n)} &= n
\end{aligned}
$$

For clarity, $d_1 = k_{d(n)}$ and $d_{d(n)} = k_1$.

Now, calculate the product of each side:

$$
d_1d_2d_3 \cdots d_{d(n)} \cdot k_1k_2k_3 \cdots k_{d(n)} = n^{d(n)}
$$

Since $d_i = k_{d(n) + 1 - i}$, the product of $d_i$’s is equal to the product of $k_i$’s, which is the product of positive divisors $p(n)$:

$$
\begin{aligned}
p(n)^2 = n^{d(n)} \\
p(n) = \sqrt{n^{d(n)}}
\end{aligned}
$$

This also implies that $n^{d(n)}$ is always a perfect square!

### Some derived problems to exercise

- The number of divisors that have an odd sum when added to their complementary divisor
- The number of coprime complementary pairs (no common divisor other than $1$)
- The number of divisors of $3n+2$ in the same form, i.e., $3x+2$
- The number of divisors of $n^2$ larger than $n$
- The number of ways $n$ can be written as the sum of several consecutive integers, e.g., $3 + 4+ 5$
- The number of ways $n$ can be written as $\frac{1}{x} + \frac{1}{y}$

## Sum of Divisors

### Sum of All

We can put a minus in front of each divisor (toggle the sign) to get the “mirrored”, negative version. So, the sum of all divisors is trivially $0$. There isn’t really much else about it.

- A fun problem to exercise
    
    What is the sum of all integer values of $x$ such that $\frac{360360}{x+360}$ is an integer?
    

### Sum of Positives

The sum of positive divisors is equivalent to summing up the expressions $p_1^{e_1'} \cdot p_2^{e_2'} \cdot p_3^{e_3'} \cdots$ for every possible combination of exponents for $0 \le e_i' \le e_i$.

Notice that for each value of $e_1'$, the combinations of the remaining exponents are exactly the same. So, we can split the sum into the following sums:

$$
\begin{aligned}
p_1^0 &\cdot (p_2^{e_2'} \cdot p_3^{e_3'} \cdots) \\
p_1^1 &\cdot (p_2^{e_2'} \cdot p_3^{e_3'} \cdots) \\
p_1^2 &\cdot (p_2^{e_2'} \cdot p_3^{e_3'} \cdots) \\
&\vdots \\
p_1^{e_1} &\cdot (p_2^{e_2'} \cdot p_3^{e_3'} \cdots) \\
\end{aligned}
$$

**The same expression inside the parentheses $p_2^{e_2'} \cdot p_3^{e_3'} \cdots$ refers to the sum of every suitable expression.**

These smaller sums sum up to:

$$
(p_1^0 + p_1^1 + \dots + p_1^{e_1})(p_2^{e_2'} \cdot p_3^{e_3'} \cdots) = \frac{p_1^{e_1 + 1} - 1}{p_1 - 1}(p_2^{e_2'} \cdot p_3^{e_3'} \cdots)
$$

> We used the shortcut formula for $1 + x + x^2 + \dots + x^k = \dfrac{x^{k+1} - 1}{x - 1}$ here. Here is how we can extract it:
> 
> 
> $$
> \begin{aligned}
> &\color{red} (-) \ 1  &\color{red} +x + x^2 + x^3 + \dots + x^k &&&&&\color{red} = S \\
> &\color{blue} (+) &\color{blue} x + x^2 + x^3 + \dots + x^k &&&\color{blue} +x^{k+1} &&\color{blue} = xS \\
> &&&&&\color{blue}+x^{k+1} \color{red}- 1 &&= (\color{blue}x \color{red}- 1)\color{green}S
> \end{aligned}
> $$
> 
> $$
> S = \frac{x^{k+1} - 1}{x - 1}
> $$
> 

What’s so nice about this split is we can apply the same to the sum inside parentheses:

$$
\frac{p_1^{e_1 + 1} - 1}{p_1 - 1}
\cdot
\begin{pmatrix}
\begin{aligned}
p_2^0 &\cdot (p_3^{e_3'} \cdot p_4^{e_4'} \cdots) \\
p_2^1 &\cdot (p_3^{e_3'} \cdot p_4^{e_4'} \cdots) \\
p_2^2 &\cdot (p_3^{e_3'} \cdot p_4^{e_4'} \cdots) \\
&\vdots \\
p_2^{e_2} &\cdot (p_3^{e_3'} \cdot p_4^{e_4'} \cdots)
\end{aligned}
\end{pmatrix}
$$

And, the next step is:

$$
\frac{p_1^{e_1 + 1} - 1}{p_1 - 1}
\cdot
\frac{p_2^{e_2 + 1} - 1}{p_2 - 1}
\cdot
\begin{pmatrix}
\begin{aligned}
p_3^0 &\cdot (p_4^{e_4'} \cdot p_5^{e_5'} \cdots) \\
p_3^1 &\cdot (p_4^{e_4'} \cdot p_5^{e_5'} \cdots) \\
p_3^2 &\cdot (p_4^{e_4'} \cdot p_5^{e_5'} \cdots) \\
&\vdots \\
p_3^{e_3} &\cdot (p_4^{e_4'} \cdot p_5^{e_5'} \cdots) \\
\end{aligned}
\end{pmatrix}
$$

If we do this till the last prime divisor, we can obtain the final formula:

$$
\frac{p_1^{e_1 + 1} - 1}{p_1 - 1}
\cdot
\frac{p_2^{e_2 + 1} - 1}{p_2 - 1}
\cdot
\frac{p_3^{e_3 + 1} - 1}{p_3 - 1}
\cdot
\frac{p_4^{e_4 + 1} - 1}{p_4 - 1}
\cdots
$$

### Problems to exercise (using the split method)

- Sum of perfect square divisors
- Sum of squares of divisors
- Product of divisors

### Implementation

The codes below calculate the sum of divisors with and without this formula. You may validate the formula with several $n$’s!

```cpp
// https://ideone.com/D2aEfj
#include <iostream>
using namespace std;
using ll = long long;

ll power(ll prime, int exponent) {
    ll res = 1;
    while (exponent--)
        res *= prime;
    return res;
}

// 1 + prime + prime^2 + ... + prime^exponent
ll calc_geometric_sum(ll prime, int exponent) {
    return (power(prime, exponent + 1) - 1) / (prime - 1);
}

ll calc_divisors_sum_without_formula(ll n) {
    ll sum = 0;
    for (int i = 1; 1ll * i * i <= n; i++) {
        if (n % i)
            continue;
        sum += i;
        if (i != n / i)
            sum += n / i;
    }
    return sum;
}

int main() {
    ll n = 10512969114312ll; // Put ll or LL to the end to specify 64-bit literal
    ll initial_n = n;
    ll sum_of_divisors_with_formula = 1;
    ll sum_of_divisors_without_formula = calc_divisors_sum_without_formula(n);
    for (int i = 2; 1ll * i * i <= n; i++) {
        if (n % i)
            continue;
        // i is a prime divisor
        int exponent = 0;
        while (n % i == 0) {
            n /= i;
            exponent++;
        }
        sum_of_divisors_with_formula *= calc_geometric_sum(i, exponent);
    }
    // n became a prime itself
    if (n > 1) {
        // Factorization includes n^1
        sum_of_divisors_with_formula *= calc_geometric_sum(n, 1);
    }
    cout << "n = " << initial_n << "\n";
    cout << "Sum of divisors w/ formula\t= " << sum_of_divisors_with_formula << "\n";
    cout << "Sum of divisors w/o formula\t= " << sum_of_divisors_without_formula << "\n";
}
```

```python
# https://ideone.com/hsETTx
n = 10512969114312

def calc_geometric_sum(prime: int, exponent: int) -> int:
    return (prime ** (exponent + 1) - 1) // (prime - 1)

def calc_divisors_sum_without_formula(n: int) -> int:
    sum = 0
    for i in range(1, n + 1):
        # i * i is faster than i**2. I first realized while testing this code!
        if i * i > n:
            break
        if n % i:
            continue
        sum += i
        if i * i != n:
            sum += n // i
    return sum

def calc_divisors_sum_with_formula(n: int) -> int:
    sum = 1
    for i in range(2, n + 1):
        if i * i > n:
            break
        if n % i:
            continue
        exponent = 0
        while n % i == 0:
            n /= i
            exponent += 1
        sum *= calc_geometric_sum(i, exponent)
    if n > 1:
        sum *= calc_geometric_sum(n, 1)
    return sum

print("n =", n)
print("Sum of divisors w/ formula\t=", calc_divisors_sum_with_formula(n))
print("Sum of divisors w/o formula\t=", calc_divisors_sum_without_formula(n))
```

### Output

```
n = 10512969114312
Sum of divisors w/ formula	= 35028084873600
Sum of divisors w/o formula	= 35028084873600
```

### [Okay, I think we got the point: prime factorization works like a charm! Now is the time to think about primes.](Sieving%20the%20Primes!%20c13f840212d8498bb3bf5381eaf78170.md)