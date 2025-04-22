## EXP : 2 IMPLEMENTATION OF PLAY FAIR CIPHER
 
 
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
 
 ## OUTPUT :
 
 ## RESULT:
