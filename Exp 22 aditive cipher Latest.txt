#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX_LENGTH 100

void encrypt(char plaintext[], int key) {
    int length = strlen(plaintext);
    
    for (int i = 0; i < length; i++) {
        if (isalpha(plaintext[i])) {
            char base = islower(plaintext[i]) ? 'a' : 'A';
            plaintext[i] = (plaintext[i] - base + key) % 26 + base;
        }
    }
}

void decrypt(char ciphertext[], int key) {
    int length = strlen(ciphertext);
    
    for (int i = 0; i < length; i++) {
        if (isalpha(ciphertext[i])) {
            char base = islower(ciphertext[i]) ? 'a' : 'A';
            ciphertext[i] = (ciphertext[i] - base - key + 26) % 26 + base;
        }
    }
}

int main() {
    char text[MAX_LENGTH];
    int key;
    
    printf("Enter the text: ");
    fgets(text, sizeof(text), stdin);
    
    printf("Enter the key (0-25): ");
    scanf("%d", &key);
    encrypt(text, key);
    printf("Encrypted text: %s\n", text);
    decrypt(text, key);
    printf("Decrypted text: %s\n", text);
    
    return 0;
}
