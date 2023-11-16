#include <stdio.h>
#include <math.h>

int main() {
    int n = 25; 
    double pi = 3.14159;
    double e = 2.71828;

    double factorial = sqrt(2 * pi * n) * pow(n / e, n);

    printf("Approximate number of possible keys: %.2lf\n", factorial);

    return 0;
}
