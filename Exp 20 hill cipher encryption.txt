#include <stdio.h>
#include <string.h>
#include <stdlib.h>
void hillEncrypt(char *message, int key[2][2]) {
    int len = strlen(message);
    if (len % 2 != 0) {
        printf("Message length should be even for Hill cipher.\n");
        return;
    }
    char encryptedMessage[len];
    memset(encryptedMessage, 0, sizeof(encryptedMessage));

    for (int i = 0; i < len; i += 2) {
        int letter1 = message[i] - 'a';
        int letter2 = message[i + 1] - 'a';
        int encryptedLetter1 = (key[0][0] * letter1 + key[0][1] * letter2) % 26;
        int encryptedLetter2 = (key[1][0] * letter1 + key[1][1] * letter2) % 26;

        // Convert the encrypted letters back to characters
        encryptedMessage[i] = encryptedLetter1 + 'a';
        encryptedMessage[i + 1] = encryptedLetter2 + 'a';
    }

    printf("Encrypted Message: %s\n", encryptedMessage);
}

int main() {
    int key[2][2] = {
        {9, 4},
        {5, 7}
    };

    char message[] = "meet me at the usual place at ten rather than eight oclock";
    for (int i = 0; message[i]; i++) {
        if (message[i] >= 'A' && message[i] <= 'Z') {
            message[i] = message[i] - 'A' + 'a';
        }
    }

    hillEncrypt(message, key);

    return 0;
}
