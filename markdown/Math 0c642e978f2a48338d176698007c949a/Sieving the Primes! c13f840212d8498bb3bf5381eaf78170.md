# Sieving the Primes!

## Until this point, we were just dealing with a single $n$. What if we were to deal with ranges?

![Sieving (actual)](https://thumbs.gfycat.com/AcclaimedRecklessAmericancrocodile-size_restricted.gif)

Sieving (actual)

## Understanding Our Limitations

Suppose we try to find the primes up to $1$ million $(10^6)$ just for fun. With our current knowledge, what could we think?

We may just iterate up to $1$ million, and check whether each integer is a prime or not using our excellent $\sqrt{n}$ method:

```cpp
// https://ideone.com/nolkPT
#include <iostream>
using namespace std;

bool is_prime(int n) {
    if (n <= 1)
        return false;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0)
            return false;
    }
    return true;
}

int main() {
    int n = 1e6;
    int n_primes = 0;
    for (int i = 1; i <= n; i++) {
        n_primes += is_prime(i);
    }
    cout << "Number of primes up to " << n << " = " << n_primes << "!\n";
}
```

The program executes in $\sim 300$ ms. It’s pretty OK, right?

Now, try with $10$ million:

```cpp
// https://ideone.com/Kkk1sA
#include <iostream>
using namespace std;
 
bool is_prime(int n) {
    if (n <= 1)
        return false;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0)
            return false;
    }
    return true;
}
 
int main() {
    int n = 1e7;
    int n_primes = 0;
    for (int i = 1; i <= n; i++) {
        n_primes += is_prime(i);
    }
    cout << "Number of primes up to " << n << " = " << n_primes << "!\n";
}
```

It executes in $\sim 7.5$ seconds.

$10$ million isn’t enough, try with $100$ million!

```cpp
// https://ideone.com/KUrddx
#include <iostream>
using namespace std;
 
bool is_prime(int n) {
    if (n <= 1)
        return false;
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0)
            return false;
    }
    return true;
}
 
int main() {
    int n = 1e8;
    int n_primes = 0;
    for (int i = 1; i <= n; i++) {
        n_primes += is_prime(i);
    }
    cout << "Number of primes up to " << n << " = " << n_primes << "!\n";
}
```

Even on local, it will feel like it won’t even execute…

It’s because the theoretical complexity is $\mathcal{O}(n \sqrt{n})$. Substitute $n = 10^8$: what you get is such an enormous number!
Remember what you learned in the first week:

> C++ can execute approximately $100$ million $(10^8)$ operations per second.
> 

So, theoretically it takes $10^4$ seconds for $n = 10^8$. It’s about $3$ hours!

But, don’t worry. We have a special kind of method to speed up the process: **Sieve of Eratosthenes**.

## Sieve of Eratosthenes

If we deal with each integer **individually**, we can find nothing faster than $n \sqrt{n}$.

Remember, what makes an integer a prime? **It is not divisible by an integer other than $1$ and smaller than itself.**

On the other hand, each composite integer has at least one such divisor. So, we know that for an integer $i > 1$, every multiple of $i$ except itself is a composite integer.

Then, what about finding composites instead of primes?

For each integer $i$ from $2$ to $n$, we can mark $2i, 3i, 4i, \dots$ as composites. There are $\left \lfloor \frac{n}{i} \right \rfloor$such multiples.

What is the total number of multiples? It is $\left \lfloor \frac{n}{2} \right \rfloor + \left \lfloor \frac{n}{3} \right \rfloor + \left \lfloor \frac{n}{4} \right \rfloor + \dots + \left \lfloor \frac{n}{n} \right \rfloor$.

Well, guess what? This is far smaller than $n^2$, or even $n \sqrt{n}$ for $n = 10^6$. Here’s why:

> The proof uses the [harmonic series](https://en.wikipedia.org/wiki/Harmonic_series_(mathematics)):
> 
> 
> $$
> \sum_{i = 1}^{\infty} \frac{1}{i} \approx \log n
> $$
> 
> We know that the inequality $\displaystyle{\frac{n}{i} \ge \left \lfloor \frac{n}{i} \right \rfloor}$holds, therefore:
> 
> $$
> n \log n > \frac{n}{1} + \frac{n}{2} + \frac{n}{3} + \frac{n}{4} + \dots + \frac{n}{n} \ge \left \lfloor \frac{n}{1} \right \rfloor + \left \lfloor \frac{n}{2} \right \rfloor + \left \lfloor \frac{n}{3} \right \rfloor + \left \lfloor \frac{n}{4} \right \rfloor + \dots + \left \lfloor \frac{n}{n} \right \rfloor
> $$
> 

Thus, the total number of multiples is less than $n \log n$, and so is the number of operations to be performed.

### Implementation

```cpp
// https://ideone.com/NyqgrL
#include <iostream>
#include <vector>
using namespace std;

vector<bool> sieve(int n) {
    // Initially, every integer is marked as prime.
    // By convention, we named the array "is_prime" instead of "is_composite".
    vector<bool> is_prime(n + 1, true);
    is_prime[0] = is_prime[1] = false; // Should mark them by hand

    // Now, start marking composites
    for (int i = 2; i <= n; i++) {
        for (int multiple = 2 * i; multiple <= n; multiple += i) { // n/i iterations
            is_prime[multiple] = false;
        }
    }
    return is_prime;
}

int main() {
    int n = 1e6;
    vector<bool> is_prime = sieve(n);
    int n_primes = 0;
    for (int i = 2; i <= n; i++) {
        n_primes += is_prime[i];
    }
    cout << "Number of primes up to " << n << " = " << n_primes << "!\n";
}
```

```python
# https://ideone.com/7CYqnz
n = 10**6

def sieve(n: int) -> "list[bool]":
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = 1
    for i in range(2, n + 1):
        for multiple in range(2 * i, n + 1, i):
            is_prime[multiple] = False
    return is_prime

is_prime = sieve(n)
n_primes = 0
for i in range(2, n + 1):
    n_primes += is_prime[i]

print("Number of primes up to", n, "=", n_primes)
```

### Output

```
Number of primes up to 1000000 = 78498!
```

```
Number of primes up to 10000000 = 664579!
```

```
Number of primes up to 100000000 = 5761455!
```

### Further Optimizations

- When $i$ is not a prime, just continue without executing the inner loop. It reduces the complexity to $n \log \log n$.
See how the code below executes in $\sim 1.2$ seconds for $n = 10^8$:
    
    
    ```cpp
    // https://ideone.com/P6Tmcg
    #include <iostream>
    #include <vector>
    using namespace std;
    
    vector<bool> sieve(int n) {
        vector<bool> is_prime(n + 1, true);
        is_prime[0] = is_prime[1] = false;
    
        for (int i = 2; i <= n; i++) {
            if (!is_prime[i]) // <- The only line added
                continue;
            for (int multiple = 2 * i; multiple <= n; multiple += i) {
                is_prime[multiple] = false;
            }
        }
        return is_prime;
    }
    
    int main() {
        int n = 1e8;
        vector<bool> is_prime = sieve(n);
        int n_primes = 0;
        for (int i = 2; i <= n; i++) {
            n_primes += is_prime[i];
        }
        cout << "Number of primes up to " << n << " = " << n_primes << "!\n";
    }
    ```
    
- Instead of $2i$, start from $i^2$.  It’s because the multiples below $i^2$ would have already been marked as composites as they all have a prime divisor less than $i$.
The code below executes in $\sim 0.9$ seconds for $n = 10^8$:
    
    
    ```cpp
    // https://ideone.com/RLkzSo
    #include <iostream>
    #include <vector>
    using namespace std;
    
    vector<bool> sieve(int n) {
        vector<bool> is_prime(n + 1, true);
        is_prime[0] = is_prime[1] = false;
    
        for (int i = 2; i <= n; i++) {
            if (!is_prime[i])
                continue;
    
            if (i * i > n) // To prevent any overflow, just break since the inner loop won't be executed anymore
                break;
    
            for (int multiple = i * i; multiple <= n; multiple += i) {
                is_prime[multiple] = false;
            }
        }
        return is_prime;
    }
    
    int main() {
        int n = 1e8;
        vector<bool> is_prime = sieve(n);
        int n_primes = 0;
        for (int i = 2; i <= n; i++) {
            n_primes += is_prime[i];
        }
        cout << "Number of primes up to " << n << " = " << n_primes << "!\n";
    }
    ```
    
- In the previous step, notice that after $i^2 > n$, the inner loop doesn’t execute at all. Remove the break condition and iterate up to $\sqrt{n}$ in the outer loop.
The code below executes in $\sim 0.6$ seconds for $n = 10^8$:
    
    
    ```cpp
    // https://ideone.com/Hs8nmx
    #include <iostream>
    #include <vector>
    using namespace std;
    
    vector<bool> sieve(int n) {
        vector<bool> is_prime(n + 1, true);
        is_prime[0] = is_prime[1] = false;
    
        for (int i = 2; i * i <= n; i++) { // Move the "i*i > n" break condition here
            if (!is_prime[i])
                continue;
    
            for (int multiple = i * i; multiple <= n; multiple += i) {
                is_prime[multiple] = false;
            }
        }
        return is_prime;
    }
    
    int main() {
        int n = 1e8;
        vector<bool> is_prime = sieve(n);
        int n_primes = 0;
        for (int i = 2; i <= n; i++) {
            n_primes += is_prime[i];
        }
        cout << "Number of primes up to " << n << " = " << n_primes << "!\n";
    }
    ```
    

You can learn more about it at [https://cp-algorithms.com/algebra/sieve-of-eratosthenes.html](https://cp-algorithms.com/algebra/sieve-of-eratosthenes.html).

## Using Sieve

We got that we can find primes up to $n$ pretty fast. Yet, do we know what we can do with them?

### Number of primes up to some integer

Take the `is_prime` boolean array. Instead of booleans, store integers and rename the array as `n_primes`, where `n_primes[i]` stores the number of primes up to $i$.

Filling `n_primes` requires the following modifications:

- In the $i^{th}$ iteration, set `n_primes[i] = n_primes[i-1]`. This is done to provide the *“up to”* functionality. The idea of “prefix sum” is used.
- If $i$ is a prime, increment `n_primes[i]`.

With the `n_primes` array, you can now answer such queries in a single operation!

### Implementation

There are two possible implementations. The first implementation keeps using `is_prime` while the other only uses `n_primes`.

```cpp
// https://ideone.com/VVaql5
#include <iostream>
#include <vector>
using namespace std;

vector<int> sieve(int n) {
    vector<bool> is_prime(n + 1, 1);
    is_prime[0] = is_prime[1] = 0;
    vector<int> n_primes(n + 1); // if you define a vector of ints without the initial value, they'll all be initialized to 0.

    for (int i = 2; i <= n; i++) {
        n_primes[i] = n_primes[i - 1] + is_prime[i];

        if (!is_prime[i])
            continue;

        // We wrote factor <= n/i instead of factor * i <= n. Convenient trick to prevent overflow!
        for (int factor = i; factor <= n / i; factor++) {
            is_prime[factor * i] = false;
        }
    }
    return n_primes;
}

int main() {
    int n = 1e8;
    vector<int> n_primes = sieve(n);
    cout << "Number of primes up to " << n << " = " << n_primes[n] << "!\n";
}
```

```cpp
// https://ideone.com/LLhWwb
#include <iostream>
#include <vector>
using namespace std;

vector<int> sieve(int n) {
    // Use n_primes as is_prime, initially. We can compute the prefix sums later!
    vector<int> n_primes(n + 1, 1);
    n_primes[0] = n_primes[1] = 0;

    for (int i = 2; i <= n; i++) {
        n_primes[i] += n_primes[i - 1];

        if (n_primes[i] == n_primes[i - 1]) // i is not a prime
            continue;

        for (int factor = i; factor <= n / i; factor++) {
            n_primes[factor * i] = 0;
        }
    }
    return n_primes;
}

int main() {
    int n = 1e8;
    vector<int> n_primes = sieve(n);
    cout << "Number of primes up to " << n << " = " << n_primes[n] << "!\n";
}
```

### Output

```
Number of primes up to 100000000 = 5761455!
```

### Factorizing faster than $\mathcal{O}(\sqrt{n})$

Previously, we visited each integer up to $\sqrt{n}$ to factorize $n$. However, we may ignore composite numbers!

> The number of primes up to an integer $x$ is approximately $\dfrac{x}{\log x}$. Refer to *[prime-counting function](https://en.wikipedia.org/wiki/Prime-counting_function)* for further details.
> 

Just use sieve to store the number of primes in an additional array, then iterate over primes up to $\sqrt{n}$.

This method is guaranteed to catch all prime divisors. If $n$ has a prime divisor greater than $\sqrt{n}$, then it will be catched after the iteration, inside the `if(n > 1)` section because its exponent must be $1$.

This method especially helps when there are queries (multiple $n$’s).

After sieving, the complexity is $\mathcal{O} \left(\frac{\sqrt{n}}{\log \sqrt{n}} \right)$ per $n$: the number of primes. There is an additional $\log n$ from the number of inner while loop iterations, but it can be negligible.

### Implementation

```cpp
// https://ideone.com/d6ZLLE
#include <bits/stdc++.h>
using namespace std;
using ll = long long;

vector<int> primes;

void sieve(int n) {
    vector<bool> is_prime(n + 1, 1);
    is_prime[0] = is_prime[1] = 0;
    for (int i = 2; i <= n; i++) {
        if (!is_prime[i])
            continue;

        primes.push_back(i);
        for (int factor = i; factor <= n / i; factor++) {
            is_prime[factor * i] = 0;
        }
    }
}

struct PrimePower {
    ll prime;
    int exponent;
};

vector<PrimePower> factorize(ll n) {
    vector<PrimePower> factorization;

    for (int prime : primes) {
        if (1ll * prime * prime > n)
            break;
        if (n % prime)
            continue;

        PrimePower p{prime, 0};
        while (n % prime == 0) {
            n /= prime;
            p.exponent++;
        }
        factorization.push_back(p);
    }

    if (n > 1) {
        factorization.push_back({n, 1});
    }

    return factorization;
}

int main() {
    ll n = 10512969114312ll;
    int sieve_bound = sqrt(n) + 5; // +5 is to overcome precision errors! (might also be +1)
    sieve(sieve_bound);            // Find the primes up to sqrt(n)
    vector<PrimePower> factorization = factorize(n);

    cout << "n = " << n << " = ";
    for (PrimePower &prime_power : factorization) {
        cout << prime_power.prime << "^" << prime_power.exponent;
        if (prime_power.prime < factorization.back().prime)
            cout << " * ";
    }
    cout << "\n";
}
```

### Output

```
n = 10512969114312 = 2^3 * 3^5 * 7^2 * 101^1 * 103^3
```

### [The idea of “sieving” isn’t over yet. There are several useful algorithms with similar complexity. Let’s take a look at them!](Sieve-Like%20Algorithms!%209eab0309506e4089af4aca4c382e6d5b.md)