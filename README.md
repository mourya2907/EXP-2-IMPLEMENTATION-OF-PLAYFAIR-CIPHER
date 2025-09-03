## EXP : 2 IMPLEMENTATION OF PLAY FAIR CIPHER
### Name : Mourya . G
### Register number : 2122224230170
 
 
 ## AIM :
 To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.
 
 
 ## ALGORITHM:
 1.	To perform Encryption :
 •	Preprocess the key:
 a.	Convert the key to lowercase.
 b.	Remove spaces and duplicate letters.
 c.	Replace 'j' with 'i'.
 2.	Generate the 5×5 key matrix (key table):
 a.	Fill in the unique characters of the key.
 b.	Fill the remaining cells with unused letters of the alphabet (except 'j').
 3.	Preprocess the plaintext:
 a.	Convert to lowercase.
 b.	Remove spaces.
 c.	Replace 'j' with 'i'.
 d.	Break into digraphs (pairs of two letters).
 If a pair has two same letters, insert 'x' between them.
 If there's a single letter left at the end, add a padding letter (like 'z').
 4.	Encrypt each digraph using these rules:
 a.	Same row: Replace each letter with the letter to its right (wrap to start if needed).
 b.	Same column: Replace each letter with the letter below it (wrap to top if needed).
 c.	Different row and column: Swap the column positions of the two letters.
 5.	Display the encrypted ciphertext.
 6.	To perform Decryption:
 •	Preprocess the ciphertext like the encryption step.
 •	Decrypt each digraph using these rules:
 a.	Same row: Replace each letter with the letter to its left (wrap to end if needed).
 
 b.	Same column: Replace each letter with the letter above it (wrap to bottom if needed).
 
 c.	Different row and column: Swap the column positions of the two letters.
 
 7.	Display the decrypted plaintext.
 
 ## PROGRAM :
 ~~~
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define SIZE 30

// Function to convert a string to lowercase
void toLowerCase(char plain[], int ps) {
    for (int i = 0; i < ps; i++) {
        if (plain[i] >= 'A' && plain[i] <= 'Z') {
            plain[i] += 32;
        }
    }
}

// Function to remove all spaces from a string
int removeSpaces(char* plain, int ps) {
    int i, count = 0;
    for (i = 0; i < ps; i++) {
        if (plain[i] != ' ') {
            plain[count++] = plain[i];
        }
    }
    plain[count] = '\0';
    return count;
}

// Function to generate the 5x5 key square
void generateKeyTable(char key[], int ks, char keyT[5][5]) {
    int i, j, k;
    // A dictionary to track characters already in the key table
    int dict[26] = {0};

    // Mark characters from the key as used
    for (i = 0; i < ks; i++) {
        if (key[i] != 'j') {
            // Use a value of 2 to mark characters from the key
            dict[key[i] - 'a'] = 2;
        }
    }
    // Mark 'j' as used to handle the i/j combination
    dict['j' - 'a'] = 1;

    i = 0;
    j = 0;

    // 1. Fill the key table with unique characters from the key
    for (k = 0; k < ks; k++) {
        if (dict[key[k] - 'a'] == 2) {
            dict[key[k] - 'a'] -= 1; // Mark as added to the table
            keyT[i][j] = key[k];
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }

    // 2. Fill the rest of the key table with the remaining alphabet
    for (k = 0; k < 26; k++) {
        if (dict[k] == 0) { // If character is not used yet
            keyT[i][j] = (char)(k + 'a');
            j++;
            if (j == 5) {
                i++;
                j = 0;
            }
        }
    }
}

// Function to search for the coordinates of two characters in the key table
void search(char keyT[5][5], char a, char b, int arr[]) {
    int i, j;

    // Replace 'j' with 'i' as per Playfair rules
    if (a == 'j') a = 'i';
    if (b == 'j') b = 'i';

    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            if (keyT[i][j] == a) {
                arr[0] = i;
                arr[1] = j;
            }
            // FIX: Changed 'else if' to 'if'. This handles digraphs with
            // the same letter (e.g., 'xx') after preparation.
            if (keyT[i][j] == b) {
                arr[2] = i;
                arr[3] = j;
            }
        }
    }
}

// Function to find the modulus with 5, ensuring the result is positive
int mod5(int a) {
    return (a % 5 + 5) % 5;
}

// Function to prepare the plaintext string for encryption
// FIX: This function is completely rewritten. The original did not handle
// digraphs with repeated letters (like in "hello"). This version inserts
// an 'x' between repeated letters and then pads with 'z' if the length is odd.
int prepare(char str[], int ptrs) {
    if (ptrs == 0) return 0;
    
    char temp[SIZE * 2];
    int new_len = 0;
    int i = 0;

    // Handle repeated letters by inserting 'x'
    while (i < ptrs) {
        temp[new_len++] = str[i];
        if (i + 1 < ptrs) {
            if (str[i] == str[i+1]) {
                temp[new_len++] = 'x';
                i++;
            } else {
                temp[new_len++] = str[i+1];
                i += 2;
            }
        } else {
            i++;
        }
    }
    
    // Pad with 'z' if the final length is odd
    if (new_len % 2 != 0) {
        temp[new_len++] = 'z';
    }
    temp[new_len] = '\0';
    
    strcpy(str, temp);
    return new_len;
}


// Function for performing the encryption
void encrypt(char str[], char keyT[5][5], int ps) {
    int i;
    int a[4];
    for (i = 0; i < ps; i += 2) {
        search(keyT, str[i], str[i + 1], a);
        if (a[0] == a[2]) { // Same row
            str[i] = keyT[a[0]][mod5(a[1] + 1)];
            str[i + 1] = keyT[a[2]][mod5(a[3] + 1)];
        } else if (a[1] == a[3]) { // Same column
            str[i] = keyT[mod5(a[0] + 1)][a[1]];
            str[i + 1] = keyT[mod5(a[2] + 1)][a[3]];
        } else { // Rectangle
            // FIX: Corrected syntax error "key T" to "keyT"
            str[i] = keyT[a[0]][a[3]];
            str[i + 1] = keyT[a[2]][a[1]];
        }
    }
}

// Function for performing the decryption
void decrypt(char str[], char keyT[5][5], int ps) {
    int i;
    int a[4];
    for (i = 0; i < ps; i += 2) {
        search(keyT, str[i], str[i + 1], a);
        if (a[0] == a[2]) { // Same row
            str[i] = keyT[a[0]][mod5(a[1] - 1)];
            str[i + 1] = keyT[a[2]][mod5(a[3] - 1)];
        } else if (a[1] == a[3]) { // Same column
            str[i] = keyT[mod5(a[0] - 1)][a[1]];
            str[i + 1] = keyT[mod5(a[2] - 1)][a[3]];
        } else { // Rectangle
            str[i] = keyT[a[0]][a[3]];
            str[i + 1] = keyT[a[2]][a[1]];
        }
    }
}

// Main function to encrypt a string using Playfair Cipher
void encryptByPlayfairCipher(char str[], char key[]) {
    char keyT[5][5];

    // Prepare Key
    int ks = strlen(key);
    toLowerCase(key, ks);
    ks = removeSpaces(key, ks);

    // Prepare Plaintext
    int ps = strlen(str);
    toLowerCase(str, ps);
    ps = removeSpaces(str, ps);
    ps = prepare(str, ps);

    generateKeyTable(key, ks, keyT);
    encrypt(str, keyT, ps);
    
    // FIX: Removed a redundant and misplaced call to toLowerCase(key, ks)
}

// Main function to decrypt a string using Playfair Cipher
void decryptByPlayfairCipher(char str[], char key[]) {
    char keyT[5][5];

    // Prepare Key
    int ks = strlen(key);
    // FIX: Added missing call to toLowerCase for the key. Without this,
    // a key with uppercase letters would generate an incorrect table.
    toLowerCase(key, ks);
    ks = removeSpaces(key, ks);

    // Prepare Ciphertext (although it's already prepared)
    int ps = strlen(str);
    toLowerCase(str, ps);
    ps = removeSpaces(str, ps);

    generateKeyTable(key, ks, keyT);
    decrypt(str, keyT, ps);
}

// Driver code
int main() {
    char str[SIZE], key[SIZE];

    printf("Simulating Playfair Cipher\n\n");

    // Key to be used
    strcpy(key, "Monopoly");
    printf("Key text: %s\n", key);

    // Plaintext to be encrypted
    strcpy(str, "NIDISH");
    printf("Plain text: %s\n", str);

    // Encrypt using Playfair Cipher
    encryptByPlayfairCipher(str, key);
    printf("Cipher text: %s\n", str);

    // Decrypt using Playfair Cipher
    decryptByPlayfairCipher(str, key);
    printf("Decrypted text: %s\n", str);

    return 0;
}
~~~

 
 ## OUTPUT :
<img width="1522" height="877" alt="Screenshot 2025-09-03 085022" src="https://github.com/user-attachments/assets/573192d4-801f-4fcb-8e6f-030f5e0600e1" />
 
 
 ## RESULT:
 The program implementing the Hill cipher for encryption and decryption has been successfully 
executed, and the results have been verified
