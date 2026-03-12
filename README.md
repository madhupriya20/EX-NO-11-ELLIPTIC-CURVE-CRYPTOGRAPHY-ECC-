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
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y


def mod_inverse(a, m):
    m0 = m
    x0, x1 = 0, 1

    if m == 1:
        return 0

    while a > 1:
        q = a // m
        t = m
        m = a % m
        a = t
        t = x0
        x0 = x1 - q * x0
        x1 = t

    if x1 < 0:
        x1 += m0

    return x1


def point_addition(P, Q, a, p):
    if P.x == Q.x and P.y == Q.y:
        lam = ((3 * P.x * P.x + a) * mod_inverse(2 * P.y, p)) % p
    else:
        lam = ((Q.y - P.y) * mod_inverse(Q.x - P.x, p)) % p

    x3 = (lam * lam - P.x - Q.x) % p
    y3 = (lam * (P.x - x3) - P.y) % p

    return Point((x3 + p) % p, (y3 + p) % p)


def scalar_multiplication(P, k, a, p):
    result = P
    k -= 1
    while k > 0:
        result = point_addition(result, P, a, p)
        k -= 1
    return result


# Input values
p = int(input("Enter the prime number (p): "))
a = int(input("Enter curve parameter a: "))
b = int(input("Enter curve parameter b: "))

gx = int(input("Enter base point Gx: "))
gy = int(input("Enter base point Gy: "))

G = Point(gx, gy)

privateA = int(input("Enter Madhu's private key: "))
privateB = int(input("Enter Priya's private key: "))

# Public keys
publicA = scalar_multiplication(G, privateA, a, p)
publicB = scalar_multiplication(G, privateB, a, p)

print(f"Madhu's public key: ({publicA.x}, {publicA.y})")
print(f"Priya's public key: ({publicB.x}, {publicB.y})")

# Shared secret
sharedA = scalar_multiplication(publicB, privateA, a, p)
sharedB = scalar_multiplication(publicA, privateB, a, p)

print(f"Shared secret computed by Madhu: ({sharedA.x}, {sharedA.y})")
print(f"Shared secret computed by Priya: ({sharedB.x}, {sharedB.y})")

if sharedA.x == sharedB.x and sharedA.y == sharedB.y:
    print("Key exchange successful. Both shared secrets match!")
else:
    print("Key exchange failed. Shared secrets do not match.")
```



## Output:
<img width="453" height="290" alt="image" src="https://github.com/user-attachments/assets/7285c6c6-f91d-4efb-828c-8d80a655990c" />


## Result:
The program is executed successfully

