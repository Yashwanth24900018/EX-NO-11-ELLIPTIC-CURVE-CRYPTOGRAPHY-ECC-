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

typedef struct {
    int x;
    int y;
} Point;

// Mod function (handles negative values)
int mod(int a, int p) {
    return (a % p + p) % p;
}

// Modular inverse (brute force – for exams)
int modInverse(int a, int p) {
    for (int i = 1; i < p; i++) {
        if (mod(a * i, p) == 1)
            return i;
    }
    return -1;
}

// Point addition on elliptic curve
Point pointAdd(Point P, Point Q, int a, int p) {
    Point R;
    int lambda;

    if (P.x == Q.x && P.y == Q.y) {
        // Point doubling
        int num = mod(3 * P.x * P.x + a, p);
        int den = modInverse(2 * P.y, p);
        lambda = mod(num * den, p);
    } else {
        // Point addition
        int num = mod(Q.y - P.y, p);
        int den = modInverse(Q.x - P.x, p);
        lambda = mod(num * den, p);
    }

    R.x = mod(lambda * lambda - P.x - Q.x, p);
    R.y = mod(lambda * (P.x - R.x) - P.y, p);

    return R;
}

// Scalar multiplication (k * P)
Point scalarMultiply(Point P, int k, int a, int p) {
    Point R = P;
    for (int i = 1; i < k; i++) {
        R = pointAdd(R, P, a, p);
    }
    return R;
}

int main() {
    int a = 2, b = 3, p = 97;

    Point G = {3, 6};  // Base point on curve

    int dA, dB;
    printf("Enter private key of User A: ");
    scanf("%d", &dA);

    printf("Enter private key of User B: ");
    scanf("%d", &dB);

    // Public keys
    Point PA = scalarMultiply(G, dA, a, p);
    Point PB = scalarMultiply(G, dB, a, p);

    // Shared secrets
    Point SA = scalarMultiply(PB, dA, a, p);
    Point SB = scalarMultiply(PA, dB, a, p);

    printf("\nPublic Parameters:");
    printf("\nCurve: y^2 = x^3 + %dx + %d mod %d", a, b, p);

    printf("\n\nPublic Keys:");
    printf("\nUser A Public Key: (%d, %d)", PA.x, PA.y);
    printf("\nUser B Public Key: (%d, %d)", PB.x, PB.y);

    printf("\n\nShared Secret:");
    printf("\nUser A Shared Key: (%d, %d)", SA.x, SA.y);
    printf("\nUser B Shared Key: (%d, %d)\n", SB.x, SB.y);

    return 0;
}
```

## Output:

[Screenshot 2026-02-26 205715.pdf](https://github.com/user-attachments/files/25579893/Screenshot.2026-02-26.205715.pdf)

## Result:
The program is executed successfully

