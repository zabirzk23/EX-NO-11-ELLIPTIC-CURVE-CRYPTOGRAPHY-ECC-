# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:

```
#include <stdio.h>
#include <stdio.h>
typedef struct { long long x, y; } Point;
long long modInv(long long a, long long m) {
 long long m0 = m, x0 = 0, x1 = 1, q, t;
 while (a > 1) {
 q = a / m; t = m; m = a % m; a = t;
 t = x0; x0 = x1 - q * x0; x1 = t;
 }
 return x1 < 0 ? x1 + m0 : x1;
}
Point add(Point P, Point Q, long long a, long long p) {
 Point R; long long λ;
 if (P.x == Q.x && P.y == Q.y)
 λ = (3 * P.x * P.x + a) * modInv(2 * P.y, p) % p;
 else
 λ = (Q.y - P.y) * modInv(Q.x - P.x, p) % p;
 R.x = (λ * λ - P.x - Q.x + p) % p;
 R.y = (λ * (P.x - R.x) - P.y + p) % p;
 return R;
}
Point mul(Point P, long long k, long long a, long long p) {
 Point R = P; k--;
 while (k--) R = add(R, P, a, p);
 return R;
}
int main() {
 long long p, a, b, privA, privB;
 Point G, pubA, pubB, sharedA, sharedB;
 printf("Enter prime p, curve params a b, base point G (x y), keys a b:\n");
 scanf("%lld %lld %lld %lld %lld %lld %lld", &p, &a, &b, &G.x, &G.y, &privA,
&privB);
 pubA = mul(G, privA, a, p);
 pubB = mul(G, privB, a, p);
 sharedA = mul(pubB, privA, a, p);
 sharedB = mul(pubA, privB, a, p);
 printf("Public A: (%lld, %lld)\n", pubA.x, pubA.y);
 printf("Public B: (%lld, %lld)\n", pubB.x, pubB.y);
 printf("Shared A: (%lld, %lld)\n", sharedA.x, sharedA.y);
 printf("Shared B: (%lld, %lld)\n", sharedB.x, sharedB.y);
 if (sharedA.x == sharedB.x && sharedA.y == sharedB.y)
 printf("Key exchange successful.\n");
 else
 printf("Key exchange failed.\n");
 return 0;
}
```


## Output:

<img width="739" height="289" alt="image" src="https://github.com/user-attachments/assets/0971de5c-01b5-4b75-99a7-e6b7bf320330" />


## Result:
The program is executed successfully

