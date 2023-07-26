# Number Theory

## GCD

Euclidean algorithm:

```python
def gcd(x, y):
    if y == 0:
        return x
    return gcd(y, x % y)
```

**Time Complexity:** `O(log(min(x, y)))`

## LCM

`lcm(x, y) = (x * y) / gcd(x, y)`

## Factors of a number

### Naive approach

Iterate over numbers from `1` to `n` and check if `n` is divisible by each.
**Time Complexity:** `O(n)`

### Improved time

Iterate over numbers from `1` to `sqrt(n)`.

```
for m=1 to m=sqrt(n):
    if n % m == 0:
        add m and n/m to set of factors
```

**Time Complexity:** `O(sqrt(n))`

## Prime numbers

### Primality check

Find factors of the number and check if there are any factors other than 1 and
the number itself. **Time Complexity:** `O(sqrt(n))`

### Sieve of eratosthenes

This can be used to:
- check if a number is prime
- list all prime numbers upto a certain value
- find all prime factors of a number

```python
def sieve(n):
    # initialize all numbers as primes
    is_prime = [True for _ in range(n + 1)]
    
    # mark 0 and 1 as non primes
    is_prime[0] = is_prime[1] = False
    
    for x in range(2, n + 1):
        # mark each multiple of x as non prime
        # start from x*x for to avoid duplicate efforts
        for y in range(x * x, n + 1, x):
            is_prime[y] = False

    # return all numbers marked as prime
    return [x for x in range(n + 1) if is_prime[x]]
```

**Time Complexity:** `O(n*log(log(n)))`  
**Space Complexity:** `O(n)`

### Prime factorization

1. Divide `n` by the smallest possible prime number, until it's no longer
   divisible.
2. Repeat for all numbers from `2` to `sqrt(n)`.
3. Since we divide `n` by the current prime number as many times as it is
   divisible, there is no chance of dividing it by any non prime number.
4. Thus, we get the distinct prime factors and their frequencies during the
   process.

```python
def factorize(n):
    for f in range(2, int(sqrt(n))):
        while n % f == 0:
            # keep track of frequency if required
            n = n // f
        # if the frequency is >=1 add f to set of factors
```

**Time Complexity:** `O(sqrt(n)*log(n))`

## Modular Arithmetic

- `(a + b) % m = ((a % m) + (b % m)) % m`
- `(a - b) % m = ((a % m) - (b % m) + m) % m`
- `(a * b) % m = ((a % m) * (b % m)) % m`
- `(a / b) % m = (a * inverse(b)) % m`, where `inverse(b) = pow(b, m-2, m)`.
  If `gcd(a, b) != 1`, `inverse(b)` does not exist.

## Binary exponentiation

```python
def bpow(n, p, mod):
    if p == 0:
        return 1
    if n == 0:
        return 0
    if p & 1 == 0:
        r = bpow(n, p >> 1, mod)
        return (r * r) % mod
    r = bpow(n, (p - 1) >> 1, mod)
    return (n * r * r) % mod
```

_The order of base cases `p == 0` and `n == 0` could be changed as required 
because from a mathematical perpective `0**0` is either undefined or 1 based 
on context. For instance, in algebra and combinatorics, it is often taken as 1._

**Time Complexity:** `O(log n)`

- If `n` and/or `mod` is large (`~10**18`), use modular binary multiplication
  within binary exponentiation.
- If `p` is very large (`>>10**18`), use Euler's theorem.

## Euler Totient Function

The _Euler Totient Function (ETF)_ of a `n` is defined as the number of `k` 
such that `1 <= k <= n` and `gcd(n, k) == 1`, i.e., count of numbers smaller 
than `n` which are co-prime with `n`.

`ETF(n) = n * (1 - 1/p_0) * (1 - 1/p_1) * ... * (1 - 1/p_m)`, 
where `p_0, p_1, ..., p_m` are the distinct prime factors of `n`.

## Euler's Theorem

`(n**p) % mod == (n ** (p % ETF(mod))) % mod`  

When `mod` is prime, `ETF(mod) = mod - 1`.

## Binary multiplication

```python
def bmul(a, b, mod):
    if b == 0 or a == 0:
        return 0
    if b & 1 == 0:
        r = bmul(a, b >> 1, mod)
        return (r + r) % mod
    r = bmul(a, (b - 1) >> 1, mod)
    return (a + r + r) % mod
```

**Time Complexity:** `O(log n)`

## Bit manipulation

- Set _b_ th bit: `n = n | (1 << b)`

- Unset _b_ th bit: `n = n & ~(1 << b)`

- Flip _b_ th bit: `n = n ^ (1 << b)`

- Check if _b_ th bit is set: `n & (1 << b) != 0`

- Count number of set bits:   
  ```python
  bits = 0
  for b in range(32):
      bits += int(n & (1 << b) != 0)
  ```
  Built-in methods/functions:
  - Python: `num.bit_count()`, where `num` is an instance of `int` class
  - C++: `__builtin_popcount(num)`, where `num` is of type `int`/`long`/`long long`  

- Check odd/even: `n & 1 == 0` => even

- Multiply by 2: `n <<= 1`

- Divide by 2: `n >>= 1`

- Upper case to lower case: `ord(U) | (1 << 5) == ord(l)` (set 5th bit), where
  `U` and `l` represent any upper and lower case character respectively.

- Lower case to upper case: `ord(l) & ~(1 << 5) == ord(U)` (unset 5th bit).

- Check if `n` is a power of 2: `n & (n - 1) == 0`

### XOR

- `a xor a = 0`. _This property is used to find a number which occurs exactly
  once in a collection of numbers that occur exactly twice._

- Associative and commutative.

- Swap two numbers `a` and `b` without any helper variable.
  ```
  a = a xor b
  b = a xor b
  a = a xor b
  ```
