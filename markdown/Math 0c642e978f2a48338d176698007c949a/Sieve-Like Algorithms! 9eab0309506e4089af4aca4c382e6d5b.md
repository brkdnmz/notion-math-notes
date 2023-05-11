# Sieve-Like Algorithms!

## It’s not only the Sieve of Eratosthenes that uses the speed advantage of iterating over multiples.

## Number of Divisors of Each Integer up to $n$

While iterating over the multiples of $i$, we can increment the number of divisors of each multiple. Thus, we can achieve a complexity of $\mathcal{O}(n \log n)$ instead of $\mathcal{O}(n \sqrt n)$.

This works because we definitely visit each divisor of every integer in the outer loop, then we visit the multiples of these divisors.

### Implementation

```cpp
// https://ideone.com/8Obksu
#include <iostream>
#include <vector>
using namespace std;

vector<int> calc_number_of_divisors_up_to(int n) {
    vector<int> n_divisors(n + 1, 0);

    for (int i = 1; i <= n; i++) {
        for (int multiple = i; multiple <= n; multiple += i)
            n_divisors[multiple]++;
    }

    return n_divisors;
}

int calc_number_of_divisors_single(int n) {
    int n_divisors = 0;
    for (int i = 1; i * i <= n; i++) {
        if (n % i)
            continue;
        n_divisors += 1 + (i * i != n);
    }
    return n_divisors;
}

int main() {
    int n = 1e7;
    vector<int> n_divisors = calc_number_of_divisors_up_to(n);
    int n_numbers_to_print = 10;
    for (int i = 0; i < n_numbers_to_print; i++) {
        int randint = rand() % n + 1; // [1, n]
        cout << "Number of divisors of " << randint << ",\n";
        cout << "\tusing sqrt(n) algorithm = " << calc_number_of_divisors_single(randint) << "\n";
        cout << "\tusing sieve = " << n_divisors[randint] << "\n";
    }
}
```

### Possible Output

```
Number of divisors of 4289384,
	using sqrt(n) algorithm = 32
	using sieve = 32
Number of divisors of 6930887,
	using sqrt(n) algorithm = 4
	using sieve = 4
Number of divisors of 1692778,
	using sqrt(n) algorithm = 4
	using sieve = 4
Number of divisors of 4636916,
	using sqrt(n) algorithm = 6
	using sieve = 6
Number of divisors of 7747794,
	using sqrt(n) algorithm = 12
	using sieve = 12
Number of divisors of 4238336,
	using sqrt(n) algorithm = 22
	using sieve = 22
Number of divisors of 9885387,
	using sqrt(n) algorithm = 8
	using sieve = 8
Number of divisors of 9760493,
	using sqrt(n) algorithm = 4
	using sieve = 4
Number of divisors of 6516650,
	using sqrt(n) algorithm = 48
	using sieve = 48
Number of divisors of 9641422,
	using sqrt(n) algorithm = 16
	using sieve = 16
```

## Storing Divisors

It is very similar to finding the number of divisors. We just maintain vectors containing divisors instead of divisor counters. However, it is not used so often due to its high memory usage.

## Even Faster Factorization by Storing Prime Divisors $(\mathcal{O}(\log n))$

Previously, we learned that $n$ has at most $\log n$ prime divisors (counting non-distinct). What if we know at least one prime divisor of $n$ already?

We can continuously divide $n$ until it becomes $1$. During this, we can store the prime divisors, and consequently, factorize $n$.

**Note that we iterate the multiples of primes while sieving. So, we can store a single prime divisor of each integer up to** $n$.

### Implementation

```cpp
// https://ideone.com/aoWvPj
#include <iostream>
#include <vector>
using namespace std;

vector<int> calc_prime_divisors(int n) {
    vector<int> prime_divisor(n + 1);
    // prime_divisor[i] is any prime divisor of i (largest for most of i's)

    for (int i = 2; i <= n; i++) {
        // if i is a prime, then prime_divisor[i] == 0
        if (prime_divisor[i])
            continue; // i is not a prime, because it has a prime divisor smaller than itself

        prime_divisor[i] = i; // assign it manually, when i is a prime

        if (1ll * i * i <= n)
            for (int multiple = i * i; multiple <= n; multiple += i) {
                prime_divisor[multiple] = i;
            }
    }
    return prime_divisor;
}

struct PrimePower {
    int prime;
    int exponent;
};

vector<PrimePower> factorize(int n, vector<int> &prime_divisor) {
    vector<PrimePower> factorization;

    while (n > 1) {
        int prime = prime_divisor[n];
        PrimePower prime_power{prime, 0};
        while (n % prime == 0) {
            n /= prime;
            prime_power.exponent++;
        }
        factorization.push_back(prime_power);
    }

    return factorization; // might be unordered!
}

void print_factorization(vector<PrimePower> &factorization) {
    int i = 0;
    for (PrimePower &prime_power : factorization) {
        i++;
        cout << prime_power.prime << "^" << prime_power.exponent;
        if (i < factorization.size())
            cout << " * ";
    }
}

int main() {
    int n = 1e8;
    vector<int> prime_divisor = calc_prime_divisors(n);
    int n_numbers_to_print = 10;
    for (int i = 0; i < n_numbers_to_print; i++) {
        int randint = rand() % n + 1; // [1, n]
        vector<PrimePower> factorization = factorize(randint, prime_divisor);
        cout << randint << " = ";
        print_factorization(factorization);
        cout << "\n";
    }
}
```

### Possible Output

```
4289384 = 617^1 * 79^1 * 2^3 * 11^1
46930887 = 67^1 * 3^3 * 25943^1
81692778 = 673^1 * 3^1 * 2^1 * 20231^1
14636916 = 1019^1 * 19^1 * 7^1 * 3^3 * 2^2
57747794 = 13^1 * 2^1 * 2221069^1
24238336 = 1297^1 * 73^1 * 2^8
19885387 = 1481^1 * 29^1 * 463^1
49760493 = 3^1 * 16586831^1
96516650 = 419^1 * 271^1 * 17^1 * 5^2 * 2^1
89641422 = 13^1 * 3^2 * 2^1 * 383083^1
```

### These are some of the algorithms worth mentioning. There are so many other similar algorithms that use the same idea, but no doubts you can implement them yourselves!

### [Let’s move on to a brand new topic: **GCD & LCM**!](GCD%20&%20LCM%20Powerful%20Twins!%20568670d56b9c4bff94bef83210c8a353.md)