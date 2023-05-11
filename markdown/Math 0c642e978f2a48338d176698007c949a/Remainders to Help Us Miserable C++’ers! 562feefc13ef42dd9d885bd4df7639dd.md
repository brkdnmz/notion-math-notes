# Remainders to Help Us Miserable C++’ers!

## If we can’t deal with huge integers, let remainders help us: *Modular Arithmetic*.

![C++’ers IRL](https://thumbs.gfycat.com/CapitalCreepyKinkajou-size_restricted.gif)

C++’ers IRL

We know what a remainder is: it is the value produced after two integers are divided. For instance, we get $2$ as the remainder when $17$ is divided by $5$, or $0$ when $369$ is divided by $123$.

There was the modulo operator `%`, particularly for finding the remainder, ignoring the division result. For the examples above, $17 \ \% \ 5 = 2$ and $369 \ \% \ 123 = 0$.

To deal with large integers, we use this operator to compute the answer *modulo* some integer $m$, where $m$ is called the *modulus*. We don’t refer to $m$ alone, instead, we often use the expression “modulo $m$”. This is why the term *modulus* isn’t used so much.

What does “$x$ modulo $m$” even mean?

**It is how we refer to the remainder itself since the remainder is the result of this modulo operation. It means the same as “remainder of $x$ divided by $m$”, “remainder when $x$ is divided by $m$”…**

> *But wait, what if $m$ is so small that even if our calculations are wrong, we get the same remainder as the true answer’s remainder?
For instance, $13$ and $18$ produce the same remainder modulo $5$, but we should’ve computed $13$ instead of $18$.*
> 

Yes, that’s so right. This is why $m$ is never that small. 😏

To decrease the probability of that happening, a large prime is chosen for $m$ such as $10^9 + 7$, $998244353$, etc.

Of course, there are so many other primes, but these two numbers have some special properties on their own, which even I don’t completely know about.

FYI, we almost always use $10^9 + 7$.

Well, so far so good. But our calculations will consist of many steps that lead to the final result, therefore many intermediate values. We can’t just find the final answer in one step, and then find the remainder. But, we should somehow always deal with small integers, as this is our actual purpose.

Then, how do we safely focus on finding the remainder?

For instance, calculating $5!$ requires several steps:

1. Start with $1$.
2. Multiply by $2$ to get $2! = 2$.
3. Multiply by $3$ to get $3! = 6$.
4. Multiply by $4$ to get $4! = 24$.
5. Finally, multiply by $5$ to get $5! = 120$.

Suppose we want to calculate $5!$ modulo $23$.

What should we actually do in previous steps to immediately obtain the remainder right after the final step, which is $120 \ \% \ 23 = 5$?

We might consider calculating the remainder at each step, and continue the calculation with it:

1. $1 \cdot 2 = 2, 2 \ \% \ 23 = 2$
2. $2 \cdot 3 = 6, 6 \ \% \ 23 = 6$
3. $6 \cdot 4 = 24, 24 \ \% \ 23 = 1$
4. $1 \cdot 5 = 5, 5 \ \% \ 23 = 5$

Hey, we actually calculated the desired result!

> *But, is it always gonna work?*
> 

To answer this, let’s discover *modular arithmetic.*

## Modular Arithmetic & Congruence

Let’s move on with our modulus $m$. Think about the integers which produce the same remainder divided by $m$. For example, when $m = 3$, each of the integers $2, 5, 8, 11, \dots$ yields a remainder of $2$ when divided by $3$. As it can be realized, the difference between adjacent numbers is always $3$. Why? Think about the division equation $n = kd + r$. These numbers are in the form of $3k + 2$, and incrementing/decrementing $k$ gives us the neighboring integers: if $k = 2$, then we have $8$; for $k = 1$ and $3$, we have the neighbors $5$ and $11$.

To specify the common property of these integers $2, 5, 8, 11, 14, \dots$, we say that “they are **congruent** modulo $3$”, and we name this relation “**congruence**”.

You can think of congruence as **the equality of remainders**, just like how we say two numbers are *equal* if their values are the same — several numbers are congruent modulo $m$ if their *remainders* are the same.

Similar to how we often compare **two** numbers when we talk about equality, we also compare **two** numbers when we talk about congruence. The relation “$a$ is congruent to $b$ modulo $m$” is notated as follows:

$$
a \equiv b \pmod m
$$

That is, the same remainder is obtained when $a$ or $b$ is divided by $m$. The $\equiv$ sign (equivalency) denotes congruence.

> Negative numbers are also involved in the congruence domain. Note that a negative integer can also be carried into the range $[0, m)$ by some number of additions, that is, its remainder is also defined.
> 

Below are some example congruences:

$$
\begin{aligned}
0 &\equiv 0 &&\pmod{21421} \\
3 &\equiv 66 &&\pmod{21} \\
-19 &\equiv 49 &&\pmod{17} \\
5 &\equiv 6 &&\pmod{1}
\end{aligned}
$$

Alright, I think we all got it.

> On the other hand, remember that $a \bmod m$ refers to the remainder itself.
> 

> An integer’s remainder is the smallest positive integer it is congruent to.
> 

Now, let’s recall when two integers $a$ and $b$ are congruent modulo $m$: **the remainder is the same when divided by $m$.**

So, we can express these numbers as $a = pm + r$ and $b = qm + r$.

Will this congruence be preserved when we subtract $m$ from / add $m$ to either $a$ or $b$ (or both)?

- If we add $m$, $m$’s coefficient ($p$ or $q$) gets incremented.
- If we subtract $m$, $m$’s coefficient gets decremented.

In both cases, the remainder $r$ remains the same, so is congruence: **they stay congruent.**

This might seem pretty useless and unimportant, just like $2 + 2 = 4$.

But, we are going to derive some very handy properties just using this observation. This is how math works… Math is just so brilliant! 🤩

We have only dealt with single integers so far. Let’s play with expressions, starting with summations.

## Summation modulo $m$

Say we have $n$ not-so-small integers $a_1, a_2, a_3, \dots, a_n$, and we want to calculate $(a_1 + a_2 + a_3 + \dots + a_n )\bmod m.$

Can we apply some operations while keeping the result (remainder) the same?

Of course! We can freely subtract $m$ from whichever $a_i$ we want (or add $m$ if it’s negative).

Let’s suppose we applied the necessary operations to every $a_i$ so that $0 \le a_i < m$ always holds.

> What we actually did is replace $a_i$’s with $a_i \bmod m$, i.e., their remainder when divided by $m$.
> 

So far so good: each $a_i$ became the smallest congruent positive integer. But the sum may get so big if we just keep adding.

Well, we can apply the same operation after each addition since this operation **always** preserves congruence.

For example, let’s try to calculate $(35 + 19 - 28 + 100 + 0 - 46 + 89) \bmod 11$.

1. First, move each integer into $[0, 11)$ by adding/subtracting $11$:
    
    $$
    \begin{aligned}
    35 \equiv 2 \pmod{11} \\
    19 \equiv 8 \pmod{11} \\
    -28 \equiv 5 \pmod{11} \\
    100 \equiv 1 \pmod{11} \\
    0 \equiv 0 \pmod{11} \\
    -46 \equiv 9 \pmod{11} \\
    89 \equiv 1 \pmod{11}
    \end{aligned}
    $$
    
2. Then, sum up these integers one by one. After each summation, if the result becomes $\ge 11$, subtract $11$. Notice that the subtraction is needed at most once because each integer is less than $11$, so their sum is less than $2 \cdot 11$:
    1. $2 + 8 = 10$
    2. $10 + 5 = 15 \to 15 \equiv 4 \pmod{11}$
    3. $4 + 1 = 5$
    4. $5 + 0 = 5$
    5. $5 + 9 = 14 \to 14 \equiv 3 \pmod{11}$
    6. $3 + 1 = 4$
3. Therefore, $(35 + 19 - 28 + 100 + 0 - 46 + 89) \bmod 11 = 4$. Let’s verify this by calculating the actual sum and its remainder:
    
    $$
    35 + 19 - 28 + 100 + 0 - 46 + 89 = 169 \to 169 \bmod 11 = 4
    $$
    

That’s all about summation. Note that subtraction summation with negatives.

Let’s continue with multiplication.

## Multiplication modulo $m$

Now, we want to calculate $a_1 \cdot a_2 \cdot a_3 \cdots a_n \bmod m$.

The same idea comes to mind: addition to/subtraction from $a_i$ (changing $a_i$ in short). But when we do that, the value of the expression does not change by just $m$. So, is it even legal?

![Source: [https://knowyourmeme.com/memes/wait-thats-illegal](https://knowyourmeme.com/memes/wait-thats-illegal)](https://i.kym-cdn.com/photos/images/facebook/001/671/387/17c.jpg)

Source: [https://knowyourmeme.com/memes/wait-thats-illegal](https://knowyourmeme.com/memes/wait-thats-illegal)

**No, Master Chief, it’s not!**

The result doesn’t change by $m$, but it changes by some multiple of $m$ because when we change $a_i$, the result changes by $k \cdot m$ where $k$ is the product of other $a_j \ (j \ne i)$.

The congruence is still preserved because changing the result by $k \cdot m$ does not affect the remainder, you can think of it as adding/subtracting $m$ several times.

So, whenever we change an $a_i$, the remainder stays the same.

> As a result, everything valid for summation is also valid for multiplication!
> 

## Summations and multiplications combined

Any complex expression involving summation, multiplication, and nested expressions (parentheses) can be expanded to the general expression of $a_1 \star a_2 \star a_3 \star \dots \star a_n$, where every $\star$ is either $+$ or $\cdot$.

This expression can be seen as a summation of several multiplications. For instance, $a_1 \cdot a_2 + a_3 + a_4 \cdot a_5 \cdot a_6$ is the summation of $M_1 =a_1 \cdot a_2$, $M_2 = a_3$, and $M_3 = a_4 \cdot a_5 \cdot a_6$.

Then, we can calculate $M_i \bmod m$ one by one, and what we are left afterward is just the summation of $3$ integers.

> As a result, the same operation of changing $a_i$ holds also for these kinds of expressions!
> 

## Division

Things get a bit complicated when division is the case.

Suppose we want to calculate $\dfrac{p}{q} \bmod m$. If we first attempt to find $p \bmod m$, then divide it by $q$, we won’t achieve the desired result. Cases where $m-1 < q$ are a nice example.

Is there really no way, though? Let’s figure it out.

Let $\dfrac{p}{q} = F = km + r$ where $r$ is the remainder (suppose $F$ is an integer).

Then, $p = qkm + qr$.

> Notice that if $m > r$, then $p \bmod qm = qr$, and dividing this by $q$ directly results in $r$ itself. This is a quite practical method, which I first saw in someone else’s code.
> 

Another method uses the equality $p \bmod m = qr \bmod m = x$. Since $x \equiv qr \pmod m$, we can definitely obtain $qr$ itself by adding several $m$’s to $x$. Formally, $x + ym = qr$ is satisfied for some positive integer $y$. So, we can just keep adding $m$ until we encounter a multiple of $q$, which we expect to be $qr$, then divide it by $q$ to get $r$. However, $qr$ might not be the first multiple of $q$ we encounter. When can this happen?

> The answer is actually a bit complicated, but you may assume you can apply this method when $q$ and $m$ are coprime. In that case, the smallest multiple encountered is always going to be $qr$.
> 

We are not restricted to these methods anyway. Note that $m$ is almost always taken $10^9 + 7$ (a prime) especially when the calculations involve division.

There is a reason: **letting [Fermat’s little theorem](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem) be applicable!**

## Fermat’s little theorem

> If $p$ is a prime, then $a^p \equiv a \pmod p$ for any integer $a$.
> 

> Moreover, we can write $a^p = kp + a$. If $p$ does not divide $a$, then $k$ is definitely divisible by $a$. So, $a^{p-1} = k'p + 1$, and we get $a^{p-1} \equiv 1 \pmod p$.
> 
> 
> > Its generalized version is [Euler’s theorem](https://en.wikipedia.org/wiki/Euler%27s_theorem): $a^{\phi (n)} = 1 \pmod n$ if $a$ and $n$ are coprime.
> > 

Using this $a^{p-1} \equiv 1 \pmod p$ congruence, we can multiply any fraction $\frac{m}{n}$ by $n^{p-1}$ and calculate $m \cdot n^{p-2} \bmod p$ to find $\frac{m}{n} \bmod p$. It’s just that $n$ must not be divisible by $p$, which is not likely to happen for $p = 10^9 + 7$. This property is referred to as “inverse modulo”:

$$
\frac{1}{n} = n^{-1} \equiv n^{p-2} \pmod p
$$

$n^{p-2}$ is called the modular inverse of $n$, because their product modulo $m$ is $1$. This is very similar to multiplicative inverse (product itself results in $1$).

> *Hey, stop there! How do we even calculate $n^{p-2} \bmod p$ for $p = 10^9 + 7$?
We can’t multiply a billion $n$’s in under a second!*
> 

Yes, that’s right… But don’t worry, there is a special method just to calculate that!

## Calculating $a^b \bmod m$ FAST: Binary Exponentiation!

When $b$ is small enough, like $10$ million, we can multiply $a$ $10$ million times since each multiplication is a single operation.

But there are going to be lots of problems where $b$ is large like $10^9$, or even huge like $10^{18}$.

What to do then?

Forget about $a^b$ itself for once. Starting with $a$, we can multiply our value by itself in a single operation:

1. $a \cdot a = a^2$ (we now know $a^2$)
2. $a^2 \cdot a^2 = a^4$ (now, we have $a^4$)
3. $a^4 \cdot a^4 = a^8$
4. $a^8 \cdot a^8 = a^{16}$
5. $a^{16}\cdot a^{16} = a^{32}$

Can you see the pattern? Since we always double the exponent, they are in the following form:

$$
\huge a^{2^{n - 1}} \cdot a^{2^{n - 1}} = a^{2^n}
$$

We start from $n = 1$, and at the $i^{th}$ step, we calculate $\Large a^{2^{i}}$. We can also denote $2^i$ with $t$ and say at the $(\log_2 t)^{th}$ step, we calculate $a^t$.

So, it takes a logarithmic number of steps to calculate $a^{t}$. In other words, its complexity is $\mathcal{O}(\log t)$.

Anyway, how do we use these powers to calculate $a^b$?

For that, we use $b$’s binary representation.

### Binary representation

Binary representation represents any integer by summation of some powers of $2$. For example, $5 = 2^2 + 2^0 = (101)_2, 29 = 2^4 + 2^3 + 2^2 + 2^0 = (11101)_2$.

To obtain the binary representation of an integer, we can continuously divide it by $2$, and whenever the remainder is $1$, we include that power of $2$ ($2^i$ after the $(i+1)^{th}$ division) in the summation (representation). In fact, the $1$’s represent these remainders.

Let’s demonstrate it with a relatively large number $314$:

$$
\begin{align}
\frac{314}{2} &= 157 \ (r = 0) \\
\frac{157}{2} &= 78 \ (r = 1) \to +2^1 \\
\frac{78}{2} &= 39 \ (r = 0) \\
\frac{39}{2} &= 19\ (r = 1) \to +2^3 \\
\frac{19}{2} &= 9 \ (r = 1) \to +2^4 \\
\frac{9}{2} &= 4 \ (r = 1) \to +2^5 \\
\frac{4}{2} &= 2 \ (r = 0) \\
\frac{2}{2} &= 1 \ (r = 0) \\
\frac{1}{2} &= 0 \ (r = 1) \to +2^8 \\
\end{align}
$$

Thus, $314 = 2^8 + 2^5 + 2^4 + 2^3 + 2^1 = (100111010)_2$.

For each power of $2$ that is present in $b$’s binary representation, we can multiply the corresponding powers of $a$ to calculate $a^b$.

We do this in parallel to the calculation of $a$’s powers. Let’s demonstrate with $b = 108 = 2^6 + 2^5 + 2^3 + 2^2$:

1. Initialize the answer to $1$. It will be $a^b$ in the end.
2. $a \cdot a = a^2$ — $2^1$ is not taken.
3. $a^2 \cdot a^2 = a^{2^2}$— $2^2$ should be taken. Multiply the answer by $a^{2^2}$. It is now $a^{2^2}$.
4. $a^{2^2} \cdot a^{2^2} = a^{2^3}$ — $2^3$ should be taken. Multiply the answer by $a^{2^3}$. It is now $a^{2^2 + 2^3}$.
5. $a^{2^3} \cdot a^{2^3} = a^{2^4}$ — $2^4$ is not taken.
6. $a^{2^4} \cdot a^{2^4} = a^{2^5}$ — $2^5$ should be taken. Multiply the answer by $a^{2^5}$. It is now $a^{2^2 + 2^3 + 2^5}$.
7. $a^{2^5} \cdot a^{2^5} = a^{2^6}$ — $2^6$ should be taken. Multiply the answer by $a^{2^6}$. It is now $a^{2^2 + 2^3 + 2^5 + 2^6}$.
8. $a^{108}$ has been calculated.

## Implementation

There might be some unfamiliar lines like `result = 1ll * result * base % mod`. `1ll` is used for overflow prevention when both `result` and `base` are 32-bit integers. We will discuss them in the next section!

```cpp
// https://ideone.com/AoHkEA
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int mod = 1e9 + 7;

int binary_exponentiation(ll base, ll exponent, bool take_exponent_mod = false) {
    // The base can also be huge. Remember the properties of multiplication modulo m:
    //   We may take base % m beforehand since we just trying to compute base * base * base * ... * base (exponent times).
    base %= mod;

    // In fact, you can take exponent % (m - 1) — remember base^(m - 1) = 1 (mod m)
    if (take_exponent_mod)
        exponent %= mod - 1;

    int result = 1;
    // Find the powers of 2 in exponent's binary representation,
    // and simultaneously, multiply base with itself (base^2, base^4, base^8,...)
    // That is, they can be done simultaneously.
    while (exponent) {
        // 2's power is taken only if current value of exponent is odd!
        if (exponent & 1) { // bitwise AND, identical to exponent % 2
            result = 1ll * result * base % mod;
        }

        exponent /= 2; // or exponent >>= 1 (right shift, i.e., shift bits one to the right, drop the last bit)

        // Multiply base with itself to get the next value in [base^2, base^4, base^8,...]
        base = 1ll * base * base % mod;
    }

    return result;
}

int main() {
    ll base = 1e18, exponent = 1e18;
    cout << base << "^" << exponent << " modulo " << mod << ": " << binary_exponentiation(base, exponent) << "\n";
    cout << base << "^" << exponent % mod << " modulo " << mod << " (took exponent mod m): " << binary_exponentiation(base, exponent, true) << "\n";
}
```

```python
# https://ideone.com/GQYCm3
def binary_exponentiation(base: int, exponent: int, mod: int, take_exponent_mod=False) -> int:
    result = 1
    while exponent:
        if exponent & 1:  # or exponent % 2
            result = result * base % mod
        exponent >>= 1  # or exponent /= 2
        base = base * base % mod
    return result

base = 10**18
exponent = 10**18
mod = 10**9 + 7

print(f"{base}^{exponent} modulo {mod}: {binary_exponentiation(base, exponent, mod)}")
print(
    f"{base}^{exponent % mod} modulo {mod} (took exponent mod m): {binary_exponentiation(base, exponent, mod, take_exponent_mod=True)}"
)

# Python has built-in binary exponentiation: pow(base, exponent, mod)!
print(f"{base}^{exponent} modulo {mod} (built-in): {pow(base, exponent, mod)}")
```

### Output

```
1000000000000000000^1000000000000000000 modulo 1000000007: 504853526
1000000000000000000^49 modulo 1000000007 (took exponent mod m): 504853526
```

![Ekran görüntüsü 2022-11-01 181704.png](Remainders%20to%20Help%20Us%20Miserable%20C++%E2%80%99ers!%20562feefc13ef42dd9d885bd4df7639dd/Ekran_grnts_2022-11-01_181704.png)

### [Now, let’s count with large numbers!](Counting%20Revisited!%20b865526ec0f94443816bdd95dcf54bcb.md)