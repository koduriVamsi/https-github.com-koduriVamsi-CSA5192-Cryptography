#include <stdio.h>
#include <openssl/dsa.h>
#include <openssl/rsa.h>
#include <openssl/pem.h>
#include <openssl/err.h>

void handle_openssl_error() {
    ERR_print_errors_fp(stderr);
    exit(EXIT_FAILURE);
}

int main() {
    // Generate DSA key pair
    DSA *dsa = DSA_new();
    if (!DSA_generate_parameters_ex(dsa, 1024, NULL, 0, NULL, NULL, NULL)) {
        handle_openssl_error();
    }
    if (!DSA_generate_key(dsa)) {
        handle_openssl_error();
    }

    // Generate RSA key pair
    RSA *rsa = RSA_generate_key(1024, RSA_F4, NULL, NULL);
    if (!rsa) {
        handle_openssl_error();
    }

    // Message to be signed
    unsigned char message[] = "Hello, world!";
    size_t message_len = sizeof(message) - 1;

    // Sign the message using DSA
    unsigned char dsa_signature1[DSA_size(dsa)];
    unsigned char dsa_signature2[DSA_size(dsa)];
    unsigned int dsa_signature_len;

    if (!DSA_sign(0, message, message_len, dsa_signature1, &dsa_signature_len, dsa)) {
        handle_openssl_error();
    }

    if (!DSA_sign(0, message, message_len, dsa_signature2, &dsa_signature_len, dsa)) {
        handle_openssl_error();
    }

    // Sign the message using RSA
    unsigned char rsa_signature1[RSA_size(rsa)];
    unsigned char rsa_signature2[RSA_size(rsa)];
    unsigned int rsa_signature_len;

    if (!RSA_sign(NID_sha1, message, message_len, rsa_signature1, &rsa_signature_len, rsa)) {
        handle_openssl_error();
    }

    if (!RSA_sign(NID_sha1, message, message_len, rsa_signature2, &rsa_signature_len, rsa)) {
        handle_openssl_error();
    }

    // Compare DSA signatures
    int dsa_signature_diff = memcmp(dsa_signature1, dsa_signature2, dsa_signature_len);
    if (dsa_signature_diff == 0) {
        printf("DSA signatures are the same.\n");
    } else {
        printf("DSA signatures are different.\n");
    }

    // Compare RSA signatures
    int rsa_signature_diff = memcmp(rsa_signature1, rsa_signature2, rsa_signature_len);
    if (rsa_signature_diff == 0) {
        printf("RSA signatures are the same.\n");
    } else {
        printf("RSA signatures are different.\n");
    }

    // Clean up
    DSA_free(dsa);
    RSA_free(rsa);

    return 0;
}
