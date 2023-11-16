#include <stdio.h>
#include <stdlib.h>
#include <time.h>
char encryptChar(char plaintext, int key) {
    if (plaintext == ' ')
        return ' '; 
    char base;
    if (plaintext >= 'A' && plaintext <= 'Z')
        base = 'A';
    else
        return plaintext;

    return ((plaintext - base + key) % 26) + base;
}

int main() {
    srand(time(NULL)); 

    char plaintext[1000];
    int key[1000];

    printf("Enter the plaintext (uppercase letters and spaces only): ");
    fgets(plaintext, sizeof(plaintext), stdin);

    printf("Generating random key...\n");
    for (int i = 0; plaintext[i] != '\0'; i++) {
        if (plaintext[i] != ' ')
            key[i] = rand() % 26;
        else
            key[i] = 0; // Use 0 for spaces
    }

    printf("Generated key: ");
    for (int i = 0; plaintext[i] != '\0'; i++) {
        printf("%d ", key[i]);
    }
    printf("\n");

    printf("Encrypted text: ");
    for (int i = 0; plaintext[i] != '\0'; i++) {
        char encryptedChar = encryptChar(plaintext[i], key[i]);
        printf("%c", encryptedChar);
    }
    printf("\n");

    return 0;
}
