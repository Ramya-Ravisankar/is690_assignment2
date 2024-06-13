

11. **Explain the difference between hashing and encoding. Provide examples from your project where each is used:**
    - **Hashing:** Example and explanation with code
    - **Encoding:** Example and explanation with code

## Project Management with Docker and CI/CD

Hashing Algorithms :
A one-way process that converts data into a fixed-length alphanumeric string, called a hash, using a mathematical function. The hash is a "digital fingerprint" that's unique to the input data and can't be reversed. Hashing is used to verify the integrity of data, and it's often used in conjunction with authentication to confirm that a message hasn't been modified.

Hashing can ensure integrity and provide authentication as well. The hash function cannot be “reverse-engineered”; that is, we can't use the hash value to discover the original data that was hashed. Thus, hashing algorithms are referred to as one-way hashes.

## Code Snippets from the project for Hashing :-
File to be referred :
[user_service.py](/app/services/user_service.py)

An example of Hashing is when storing passwords (hashed passwords).

[Hashing_Example_from_Project](/screenshots/Question11/Q11_Hashing_Example.png)


Encoding:
A reversible process that transforms data from one format to another, such as converting written language into a digital format. Encoding doesn't require keys, but it does require an algorithm to decode the transformed data. The main purpose of encoding is to make data readable by most systems or usable by external processes, and it's used to ensure the integrity and usability of data


### Code Snippets from the project for Encoding :-

File to be referred :
[user_routes.py](/app/routers/user_routes.py)

An example of when to use encoding is when generating a JWT token for a user login.

[Encoding_Example_from_Project](/screenshots/Question11/Q11_Encoding_Example.png)

[Return back to answer.md](/answer.md)