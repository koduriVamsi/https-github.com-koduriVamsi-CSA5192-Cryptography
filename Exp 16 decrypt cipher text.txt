#include <stdio.h>
#include <string.h>

void decryptCaesar(char ciphertext[], int shift) {
    int i = 0;
    while (ciphertext[i] != '\0') {
        char c = ciphertext[i];

        if (c >= 'a' && c <= 'z') {
            c = 'a' + (c - 'a' - shift + 26) % 26;
        } else if (c >= 'A' && c <= 'Z') {
            c = 'A' + (c - 'A' - shift + 26) % 26;
        }

        printf("%c", c);
        i++;
    }
}

int main() {
    char ciphertext[] = "your encrypted text here";
    int shift = 3; 

    printf("Decrypted message: ");
    decryptCaesar(ciphertext, shift);
    printf("\n");

    return 0;
}
