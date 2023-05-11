# Tips, Tricks & Examples!

## There are helpful tricks that make our lives easier, which we can use while coding math stuff.

![Pam (The Office)](https://media.tenor.com/q_uUfbylCIsAAAAC/wink-i-got-you.gif)

Pam (The Office)

## Operator Precedence

In programming languages, some operators have precedence over others, that is, **their operations are performed first.**

For instance, multiplication precedes (is done before) addition and subtraction, and so does division: Firstly multiplication is performed, then addition.

To manipulate the precedence, we use parentheses `(...)`: We can compute $36$ by `4 * (5 + 3 + 1)`. Therefore, we don’t actually need to know which operator precedes which — we can just use parentheses. However, using parentheses everywhere is sometimes boring and causes dirtiness, at least in my opinion. So, knowing precedence is pretty useful.

You can see the precedences of operators in C++ here: [https://en.cppreference.com/w/cpp/language/operator_precedence](https://en.cppreference.com/w/cpp/language/operator_precedence).

When several operators have the same precedence, their *associativity* is taken into account. Associativity just stands for in which direction an expression involving several operators is computed.

You can see below the binary arithmetic operators (a *binary* operator has $2$ operands) that are associated from left to right:

![[https://en.cppreference.com/w/cpp/language/operator_precedence](https://en.cppreference.com/w/cpp/language/operator_precedence)](Tips,%20Tricks%20&%20Examples!%20ad7513c167f548f79987a72f0a833d62/Ekran_grnts_2022-11-01_184625.png)

[https://en.cppreference.com/w/cpp/language/operator_precedence](https://en.cppreference.com/w/cpp/language/operator_precedence)

Notice that multiplication, division, and modulo (remainder) operations have the same precedence of $5$. Therefore, `3 * 4 % 5 * 8 % 11` is equivalent to `(((3 * 4) % 5) * 8) % 11`, which equals $5$.

## Reference as Result: *Assignment Operators*

All operators can be considered functions. For instance, `a * b` is just like `multiply(a, b)`.

Functions can take parameters’ references. A reference of a variable is the same variable but named differently. If you mutate a reference (change its value), you also change the variable it refers to. A reference to a variable typed `t` should be defined starting with `t&`. For `int`, it is `int&`.

You may see the exemplary codes below:

```cpp
int a = 0;
int& b = a;
b += 5; // a = b = 5
b *= 3; // a = b = 15
b -= 7; // a = b = 8
```

```cpp
void mul(int& a, int b){
	a *= b;
}

int a = 6;
mul(a, 3); // a is now 18
```

Some operators don’t return the result of the operation, but **they return a reference to the left operand itself**.

Most of them are *assignment operators.* They are often used for convenient coding, but their returned references can also be used for chaining operations.

Some examples are shown in the code below:

```cpp
// https://ideone.com/5l0e2Z
#include <iostream>
using namespace std;
 
int main() {
	int a = 45;
	(((a -= 7) /= 4) *= 5) %= 11;
	cout << a << "\n";
}
```

- After `a -= 7`, `a` becomes $38$.
- The left-hand side of the operation `/= 4` is `a -= 7` which returns a reference to `a` after subtracting $7$. Therefore, `38 / 4` is performed: `a` becomes $9$.
- After `a *= 5` (where $a = 9$), `a` becomes $45$.
- After `a %= 11`, `a` becomes $1$.

It is more used in mod calculations, such as `(a *= b) %= mod`.

## Overflow: How to Perform Mod Calculations Safely

In C++, we may just use `long long`s to prevent possible overflows that could be caused by using `int`s.

An `int` is bounded by $2^{31} - 1 = 2147483647 \approx 2 \cdot 10^9$, whereas `long long` can be up to $2^{63} - 1 = 9223372036854775807 \approx 9 \cdot 10^{18}$. It’s because they are assigned $32$ and $64$ bits in the memory (the first bit is for sign).

Since they are bounded, they cannot have a larger value than their upper bound. If an operation results in a larger value, **overflow** occurs, just like how excess water overflows when a water glass is overfilled:

![https://thumbs.dreamstime.com/b/overflowing-mineral-water-transparent-glass-drops-bu-overflowing-mineral-water-transparent-glass-drops-119980903.jpg](https://thumbs.dreamstime.com/b/overflowing-mineral-water-transparent-glass-drops-bu-overflowing-mineral-water-transparent-glass-drops-119980903.jpg)

However, overflow doesn’t work the same way in programming: the glass doesn’t remain filled.

When integers overflow, they go back to the **lower bound**. For `int`, it is $-2^{31}$ and for `long long`, it is $-2^{63}$. (Notice that there are $2^{32}$ and $2^{64}$ integers within the boundaries, respectively.)

See the code below for an example:

```cpp
// https://ideone.com/2fsoi7
#include <iostream>
using namespace std;
 
int main() {
	int a = 2147483647;
	cout <<"a: " << a << "\n";
	cout <<"a + 1: " << a + 1 << "\n";
	cout <<"a + 2: " << a + 2 << "\n";
	cout <<"2a: " << 2 * a << "\n";
	cout <<"a + a: " << a + a << "\n";
	cout <<"10a: " << 10 * a << "\n";
}
```

### Output

```
a: 2147483647
a + 1: -2147483648
a + 2: -2147483647
2a: -2
a + a: -2
10a: -10
```

As mentioned previously, we choose $10^9 + 7$ as the modulus on algoleague. Other platforms such as Codeforces, HackerRank, and AtCoder also use it as the default modulus value. $998244353$ can also be used.

Therefore, performing modulo operations results within $[0, 10^9 + 7)$. If we take such three integers $a, b, c$ and $\bmod = 10^9 + 7$:

These might overflow:

```cpp
// a b, c are both ints
a * b % mod;
(a + b + c) % mod;
a * b * c % mod;
a * a % mod;
a * b * a * b * a * b % mod;
```

Recall operator precedence: multiplication and modulo have the same precedence.

- For `a * b % mod`, first, `a * b` is performed. Overflow might occur right there because they are both `int`s whereas the result might exceed the `int` bound as $a, b < 10^9 + 7$.
- For `(a + b + c) % mod`, first, `a + b` is performed. It’s not going to overflow since the result can be at most $2 \cdot (\bmod - 1) = 2 \cdot 10^9 + 12$.
Then, `+ c` is performed. Here, the actual result can be at most $3 \cdot (\bmod - 1)$ which surely exceeds the `int` bound.

The next examples are similar to these.

These are guaranteed to not overflow:

```cpp
(a + b) % mod;
((a + b) % mod + c) % mod;
1ll * a * b % mod;
1LL * a * b % mod;
(long long)a * b % mod;
1ll * a * b % mod * c % mod;
1ll * a * b % mod * (1ll * b * c % mod);
1ll * a * b % mod * b % mod * b % mod * b % mod * b % mod;
```

- `(a + b) % mod` cannot overflow, it is explained on the left.
- The result of `(a + b) % mod` is also less than `mod`. Therefore, `((a + b) % mod + c) % mod` is similar to the first example.
- `1ll` is a literal just like `1, 2, 3` standing for “$1$ long long”, i.e., $64$-bit $1$.
Any expression containing `long long`s and `int`s can be treated as if each element were `long long`. Therefore, `1ll * a * b` won’t overflow, and its mod can be taken safely.
- `1LL` is an alias for `1ll`. They are identical.
- Instead of adding a factor of `1ll`, we can cast one of the variables to `long long` such as `(long long) a`.
- `1ll * a * b % mod` results in less than `mod`, and the result is also 64-bit. So, multiplying by `c` won’t hurt.

The remaining examples can be explained with these.

Once you understand these, it’s all safe!

### [And now, we’re done!](Congratulations!%20%F0%9F%8E%89%F0%9F%A5%B3%20c8f7f03153594a099df55802e4f1131a.md)