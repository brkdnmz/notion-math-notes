# Counting Revisited!

## We have now modulo by our side! Let’s deal with big guys, shall we?

![Dr. Strange vs. Thanos (Avengers: Infinity War)](https://media.tenor.com/uaqdp9Tl4-YAAAAd/doctor-strange.gif)

Dr. Strange vs. Thanos (Avengers: Infinity War)

Without the modulo operation, we don’t have a feasible option in C++.

Using strings to represent unbounded integers and implementing arithmetic operations on strings is always an option, but this would be computationally expensive as numbers get enormous.

So, we just use remainders!

Let’s see how to calculate the elementary stuff regarding counting modulo $m = 10^9 + 7$.

> You can always calculate stuff or verify your results at [WolframAlpha](https://www.wolframalpha.com/)!
> 

## Factorials

$n! = 1 \cdot 2 \cdots n$ is just the product of $n$ integers. We can iteratively calculate $n!$ by multiplying these integers one by one.

## Implementation

```cpp
// https://ideone.com/OLseOi
#include <bits/stdc++.h>
using namespace std;

const int mod = 1e9 + 7;

int mul(int a, int b) {
    return 1ll * a * b % mod;
}

int factorial(int n) {
    int res = 1;
    for (int i = 1; i <= n; i++) {
        res = mul(res, i);
    }
    return res;
}

int factorial_recursive(int n) {
    if (!n)
        return 1;
    return mul(n, factorial_recursive(n - 1));
}

void print_factorials(int bound) {
    int n_examples = 10;
    for (int i = 0; i <= n_examples; i++) {
        int n = rand() % (bound + 1);
        cout << n << "!:\n";
        cout << "\t" << factorial(n) << " (iterative)\n";
        cout << "\t" << factorial_recursive(n) << " (recursive)\n";
    }
}

int main() {
    int bound = 1e7;
    print_factorials(bound);
}
```

```python
# https://ideone.com/rskzqd
from random import randint
from sys import setrecursionlimit

setrecursionlimit(10**6)

MOD = 10**9 + 7

def factorial(n: int) -> int:
    res = 1
    for i in range(1, n + 1):
        res *= i
        res %= MOD
    return res

def factorial_recursive(n: int) -> int:
    if not n:
        return 1
    return n * factorial_recursive(n - 1) % MOD

def print_factorials(bound: int) -> int:
    n_examples = 10
    for i in range(n_examples):
        n = randint(0, bound)
        print(f"{n}!:")
        print(f"{factorial(n)} (iterative)")
        print(f"{factorial_recursive(n)} (recursive)")

bound = 10**5
print_factorials(bound)
```

### Possible Output

```
4289203!:
	310770995 (iterative)
	310770995 (recursive)
6930802!:
	273650626 (iterative)
	273650626 (recursive)
1692609!:
	416863105 (iterative)
	416863105 (recursive)
4636744!:
	65879630 (iterative)
	65879630 (recursive)
7747598!:
	193933211 (iterative)
	193933211 (recursive)
4238293!:
	466593853 (iterative)
	466593853 (recursive)
9885315!:
	642045382 (iterative)
	642045382 (recursive)
9760328!:
	225663923 (iterative)
	225663923 (recursive)
6516590!:
	656888815 (iterative)
	656888815 (recursive)
9641303!:
	899421366 (iterative)
	899421366 (recursive)
5202260!:
	688271574 (iterative)
	688271574 (recursive)
```

## Binomial Coefficients

$$
⁍
$$

See it as a fraction $\frac{p}{q}$. We can use $q^{m-1} \equiv 1 \pmod m$ here, where $q$ is the denominator $(n-k)! \cdot k!$ which is coprime with $m = 10^9 + 7$ for $k \le n \le 10^7$, because both $n - k$ and $k$ are less than $m$. So, we are multiplying $\frac{p}{q}$ by $q^{m-1}$ to just calculate $p \cdot q^{m-2}$. Recall that we can use binary exponentiation here.

There are a few important tricks used for one of the best implementations:

- We often precalculate $i!$’s for $i \in [1, n]$ (store in an array) to then calculate a binomial coefficient in $\mathcal{O}(\log m)$ time whenever needed (because of binary exponentiation). Because, problems often require lots of binomial coefficients with different $(n, k)$’s.
I strongly recommend you todo precalculation like so. It won’t harm, and is pretty advantageous.
- Note that $q = (n-k)! \cdot k!$. To calculate $q^{m-2}$, we can either calculate $(q \bmod m)^{m-2}$ or $\left[\left((n-k)!^{m-2} \bmod m \right) \cdot \left(k!^{m-2} \bmod m \right) \right] \bmod m$.
This means we can choose to separately calculate the factorials’ powers to $m-2$, then multiply them modulo $m$. So, $i!^{m-2}$’s can be precalculated in $\mathcal{O}(n \log m)$. This optimizes the precalculation explained previously to $\mathcal{O}(1)$, because no binary exponentiation is needed after this extra precalculation.
    
    > $i!^{m-2}$ is also called *inverse factorial*!
    > 
- Inverse factorials can even be precalculated in $\mathcal{O}(n + \log m) \approx \mathcal{O}(n)$, considering the following property:
    
    $$
    (i+1)\cdot(i+1)!^{m-2} \equiv (i+1)^{m-1} \cdot i!^{m-2} \equiv i!^{m-2} \pmod m
    $$
    
    It actually represents $\dfrac{i+1}{(i+1)!} = \dfrac{1}{i!}$.
    
    So, we can start by calculating $n!^{m-2}$ in $\mathcal{O}(\log m)$ using the already calculated $n!$, then calculate $i!$’s from $n-1!$ to $0!$ in $\mathcal{O}(1)$ per each.
    

As a result, factorials and their inverses can be precalculated in $\mathcal{O}(n)$, then a single binomial coefficient can be calculated in $\mathcal{O}(1)$.

## Implementation

```cpp
// https://ideone.com/HFQx8j
#include <bits/stdc++.h>
using namespace std;

const int N = 1e6 + 5;
const int mod = 1e9 + 7;
int f[N], inv_f[N];

int mul(int a, int b) {
    return 1ll * a * b % mod;
}

// base^(mod - 2)
int invexp(int base) {
    int result = 1;
    int exponent = mod - 2;
    while (exponent) {
        if (exponent & 1)
            result = mul(result, base);
        exponent >>= 1;
        base = mul(base, base);
    }
    return result;
}

void precalculate() {
    f[0] = 1;
    for (int i = 1; i < N; i++) {
        f[i] = mul(f[i - 1], i);
    }

    inv_f[N - 1] = invexp(f[N - 1]);
    for (int i = N - 2; i >= 0; i--) {
        inv_f[i] = mul(i + 1, inv_f[i + 1]);
    }
}

void print_binomial_coefficients(int upper_bound) {
    // An example so-called "lambda" function, similar to those in Python.
    // I used it to just let you know about them.
    auto C = [&](int n, int k) {
        if (n < k)
            return 0;
        return mul(f[n], mul(inv_f[n - k], inv_f[k]));
    };

    int n_examples = 10;
    for (int i = 0; i < n_examples; i++) {
        int n = rand() % (N + 1), k = rand() % (N + 1);
        cout << "C(" << n << ", " << k << ") mod (1e9 + 7): " << C(n, k) << "\n";
    }
}

int main() {
    // To make program generate different sequences of random integers for each run.
    srand(time(0));
    precalculate();
    int upper_bound = 1e6;
    print_binomial_coefficients(upper_bound);
}
```

### Possible Output

```
C(617265, 33507) mod (1e9 + 7): 318042414
C(233987, 572488) mod (1e9 + 7): 0
C(484668, 290576) mod (1e9 + 7): 303419600
C(724553, 553872) mod (1e9 + 7): 541519287
C(15596, 918428) mod (1e9 + 7): 0
C(17365, 300144) mod (1e9 + 7): 0
C(178273, 507821) mod (1e9 + 7): 0
C(51491, 532224) mod (1e9 + 7): 0
C(825064, 727544) mod (1e9 + 7): 19964359
C(24862, 689311) mod (1e9 + 7): 0
```

### Actual Result

![Untitled](Counting%20Revisited!%20b865526ec0f94443816bdd95dcf54bcb/Untitled.png)

---

These precalculation tricks are only feasible for small $n$’s bounded by some number like $10^7$ (think about the memory usage also).

We can do something else without precalculation for $n \le 10^{18}, |n - k| \le 10^7$. Notice how we can simplify the formula as follows when we divide $n!$ by $(n - k)!$ (or $k!$ if it’s larger):

$$
\binom{n}{k} = \frac{n \cdot (n-1) \cdots (n-k+1)}{k!}
$$

This time, it’s straightforward — calculate $n \cdot (n-1) \cdots (n-k+1) \cdot k!^{m-2}$.

## Implementation

```cpp
// https://ideone.com/5V5FFT
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

const int mod = 1e9 + 7;

int mul(int a, int b) {
    return 1ll * a * b % mod;
}

// base^(mod - 2)
int invexp(int base) {
    int result = 1;
    int exponent = mod - 2;
    while (exponent) {
        if (exponent & 1)
            result = mul(result, base);
        exponent >>= 1;
        base = mul(base, base);
    }
    return result;
}

int C(ll n, ll k) {
    if (n < k)
        return 0;
    // n * (n-1) * (n-2) * ... * (n-k+1) / k!
    k = min(k, n - k); // to guarantee k is small
    int numerator = 1, denominator = 1;
    for (ll i = n; i > n - k; i--) {
        numerator = mul(numerator, i % mod);
        denominator = mul(denominator, (i - (n - k)) % mod);
    }
    return mul(numerator, invexp(denominator));
}

void print_binomial_coefficients(int upper_bound) {

    int n_examples = 10;
    for (int i = 0; i < n_examples; i++) {
        ll n = 1ll * rand() * rand();
        bool k_is_big = rand() % 2;
        ll k = rand() % (upper_bound + 1);
        if (k_is_big)
            k = n - rand() % (upper_bound + 1);
        cout << "C(" << n << ", " << k << ") mod (1e9 + 7): " << C(n, k) << "\n";
    }
}

int main() {
    // To make program generate different sequences of random integers for each run.
    srand(time(0));
    int upper_bound = 1e6;
    print_binomial_coefficients(upper_bound);
}
```

### Possible Output

```
C(901955771036851310, 901955771036471535) mod (1e9 + 7): 32639885
C(3500670608685442500, 3500670608684988579) mod (1e9 + 7): 342476509
C(76888276043538610, 350812) mod (1e9 + 7): 250797021
C(1674628222247390508, 683976) mod (1e9 + 7): 973246379
C(18888589190746904, 18888589189758093) mod (1e9 + 7): 273027683
C(236783550266238432, 208235) mod (1e9 + 7): 452157557
C(2323159114943801680, 2323159114942805621) mod (1e9 + 7): 70432944
C(25036035499929048, 897334) mod (1e9 + 7): 625856240
C(143831834755871313, 143831834755451183) mod (1e9 + 7): 906822486
C(556445638826771468, 58006) mod (1e9 + 7): 122432077
```

### Actual Result

![Untitled](Counting%20Revisited!%20b865526ec0f94443816bdd95dcf54bcb/Untitled%201.png)

### [This finalizes our math journey. However, there are some quite useful tips and tricks left. Let’s see!](Tips,%20Tricks%20&%20Examples!%20ad7513c167f548f79987a72f0a833d62.md)