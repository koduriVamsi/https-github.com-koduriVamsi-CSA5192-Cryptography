#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Function to remove spaces and convert to uppercase
void preprocessMessage(char *message) {
    int len = strlen(message);
    for (int i = 0; i < len; i++) {
        if (message[i] == ' ')
            strcpy(&message[i], &message[i + 1]);
        message[i] = toupper(message[i]);
        if (message[i] == 'J')
            message[i] = 'I';
    }
}

// Function to generate the Playfair matrix
void generatePlayfairMatrix(char matrix[5][5], const char *key) {
    int keyLen = strlen(key);
    int index = 0;
    int used[26] = {0};

    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (index < keyLen) {
                char c = toupper(key[index]);
                if (used[c - 'A'] == 0 && c != 'J') {
                    matrix[i][j] = c;
                    used[c - 'A'] = 1;
                    index++;
                }
            }

            if (index >= keyLen) {
                for (char c = 'A'; c <= 'Z'; c++) {
                    if (used[c - 'A'] == 0 && c != 'J') {
                        matrix[i][j] = c;
                        used[c - 'A'] = 1;
                        break;
                    }
                }
            }
        }
    }
}

// Function to find the coordinates of a character in the Playfair matrix
void findCharCoordinates(const char matrix[5][5], char c, int *row, int *col) {
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (matrix[i][j] == c) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Function to perform encryption using Playfair cipher
void encryptPlayfair(char matrix[5][5], const char *message, char *cipher) {
    int len = strlen(message);
    int pairs = len / 2 + len % 2;

    preprocessMessage(cipher);

    // Add 'X' to the end if the message length is odd
    if (len % 2 != 0) {
        strcat(cipher, "X");
        len++;
    }

    // Encrypt pairs of characters
    for (int i = 0; i < pairs; i++) {
        char first = cipher[2 * i];
        char second = cipher[2 * i + 1];
        int row1, col1, row2, col2;

        findCharCoordinates(matrix, first, &row1, &col1);
        findCharCoordinates(matrix, second, &row2, &col2);

        if (row1 == row2) {
            cipher[2 * i] = matrix[row1][(col1 + 1) % 5];
            cipher[2 * i + 1] = matrix[row2][(col2 + 1) % 5];
        } else if (col1 == col2) {
            cipher[2 * i] = matrix[(row1 + 1) % 5][col1];
            cipher[2 * i + 1] = matrix[(row2 + 1) % 5][col2];
        } else {
            cipher[2 * i] = matrix[row1][col2];
            cipher[2 * i + 1] = matrix[row2][col1];
        }
    }
}

int main() {
    char key[] = "MFHIJKUNOPQZVWXYELARGS";
    char matrix[5][5];
    char message[] = "Must see you over Cadogan West. Coming at once.";
    char cipher[100];

    generatePlayfairMatrix(matrix, key);
    encryptPlayfair(matrix, message, cipher);

    printf("Original Message: %s\n", message);
    printf("Encrypted Message: %s\n", cipher);

    return 0;
}
