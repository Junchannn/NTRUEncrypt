# NTRUEncrypt

A toy Python implementation of the NTRUEncrypt public-key cryptosystem.

This project implements the core NTRU workflow over the convolution ring:

```text
R = Z_q[x] / (x^N - 1)
```

It is mainly intended for learning and experimentation, not production cryptographic use.

## Overview

NTRUEncrypt is a lattice-based public-key encryption scheme built from polynomial arithmetic over quotient rings.

At a high level, the scheme works as follows:

1. Generate two small private polynomials `f` and `g`.
2. Compute inverses of `f` modulo `p` and modulo `q`.
3. Build the public key `h` from `g` and the inverse of `f`.
4. Encrypt a message polynomial using a random small polynomial `r`.
5. Decrypt by multiplying with the private key `f`, center-lifting, and reducing modulo `p`.

The implementation follows the algorithmic structure from:

> Jeffrey Hoffstein, Jill Pipher, Joseph H. Silverman,  
> *NTRU: A Ring-Based Public Key Cryptosystem*

## Project Structure

```text
NTRUEncrypt/
├── NTRU.py       # Main NTRU key generation, encryption, and decryption logic
├── poly.py       # Polynomial arithmetic over modular rings
├── common.py     # Helper functions for random polynomials and message conversion
└── README.md     # Project documentation
```

## Features

- Toy NTRUEncrypt implementation in Python
- Key generation with ternary private polynomials
- Polynomial arithmetic over modular rings
- Multiplication in the convolution ring `Z_q[x] / (x^N - 1)`
- Polynomial inversion modulo `x^N - 1`
- Hensel-lifting-style inverse computation for prime-power moduli
- Block-based bit-list encryption and decryption
- Basic image-byte-to-bit test example
- Karatsuba-style polynomial multiplication routine

## Dependencies

The code uses the following Python packages:

- `numpy`
- `sympy`
- `pycryptodome`
- `tqdm`
- `Pillow`

Install them with:

```bash
pip install numpy sympy pycryptodome tqdm pillow
```


## Example Usage

```python
from NTRU import NTRU

# Example toy parameters
N = 107
p = 3
q = 128
df = 14
dg = 12
d = 5

# Message as a list of bits
message = [1, 0, 1, 1, 0, 1, 0, 0]

ntru = NTRU(N, p, q, df, dg, d)

public_key = ntru.key_gen()

ciphertext = ntru.encrypt(message, block_size=96)
decrypted = ntru.decrypt(ciphertext, block_size=96)

print("Original message: ", message)
print("Decrypted message:", decrypted)

assert message == decrypted
```

## Running the Test

The bottom of `NTRU.py` contains a simple test routine. It reads an image file named:

```text
lena.png
```

Then converts part of the image bytes into a bit list, encrypts the bits, decrypts them, and checks correctness.

To run it:

```bash
python NTRU.py
```

Make sure `lena.png` exists in the project directory before running the default test.

## Parameters

The example test uses:

```python
N = 107
p = 3
q = 128
df = 14
dg = 12
d = 5
```

where:

- `N` is the ring dimension
- `p` is the plaintext modulus
- `q` is the ciphertext modulus
- `df` controls the number of non-zero coefficients in `f`
- `dg` controls the number of non-zero coefficients in `g`
- `d` controls the number of non-zero coefficients in the random encryption polynomial `r`

These are toy parameters for demonstration only.

## Important Notes

This project is educational and experimental.

It should not be used for real-world cryptographic security without significant additional work, including:

- parameter validation
- constant-time implementation
- secure randomness
- padding and message encoding design
- decryption-failure analysis
- side-channel resistance
- security review against modern NTRU standards

The current padding method uses the value `2` to pad bit lists before encryption. This is simple and useful for testing, but it is not secure padding.

## License

No license has been specified yet.
