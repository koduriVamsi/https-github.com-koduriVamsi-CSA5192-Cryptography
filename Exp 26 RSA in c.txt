#include <stdio.h>
int gcd(int a, int b) {
    if (b == 0)
        return a;
    return gcd(b, a % b);
}
long long modExp(long long a, long long b, long long m) {
    long long result = 1;
    a = a % m;
    while (b > 0) {
        if (b & 1) {
            result = (result * a) % m; 
        }
        b >>= 1;
        a = (a * a) % m; 
    }
    return result;
}
int main() {
    int p, q, n, phi, e, d, m, encrypted, decrypted;
    printf("enter the value of p = ");
    scanf("%d", &p);
    printf("enter the value of q=  ");
    scanf("%d", &q);
    printf("enter the plaintext (integer) = ");
    scanf("%d", &m);
    n = p * q;
    phi = (p - 1) * (q - 1);
    e = 2;
    while (e < phi) {
        if (gcd(e, phi) == 1)
            break;
        e++;
    }
    d = 1;
    while ((d * e) % phi != 1) {
        d++;
    }
    encrypted = modExp(m, e, n);
    printf("encrypted= %d\n", encrypted);
    decrypted = modExp(encrypted, d, n);
    printf("decrypted= %d\n", decrypted);
    return 0;
}
