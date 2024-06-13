
10. **How do you ensure the security of user passwords in your project? Discuss the hashing algorithm used and any additional security measures implemented.**

## Answer:
Hashing is the process of transforming any given key or a string of characters into another value. This is usually represented by a shorter, fixed-length value or key that represents and makes it easier to find or employ the original string. A hash function generates new values according to a mathematical hashing algorithm.

Salting - BCrypt internally generates a random salt while encoding passwords and store that salt along with the encrypted password. Hence, it is obvious to get different encoded results for the same string. But one common thing is that everytime it generates a String of length 60

<span style="color: blue;"The user password security is our fastapi application is ensured through the hashing algorithm. In our project from the below code snippet, the hash_password function uses bcrypt hash algorithm.
When the user enters their password into the login form, the password is given to a hashing algorithm that converts the text into a special string that must match the hashed password value of the user stored in the database , to authenicate the user into the system.</span>

### Code Snippets from the project
[user_service.py](/app/services/user_service.py)
```
 validated_data['hashed_password'] = hash_password(validated_data.pop('password'))
            new_user = User(**validated_data)
            new_nickname = generate_nickname()
            while await cls.get_by_nickname(session, new_nickname):
                new_nickname = generate_nickname()
            new_user.nickname = new_nickname
            user_count = await cls.count(session)
            new_user.role = UserRole.ADMIN if user_count == 0 else UserRole.ANONYMOUS
            if new_user.role == UserRole.ADMIN:
                new_user.email_verified = True
```

Pros of hashing ::
Hashing has applications in various fields such as cryptography, computer science and data management. Some common uses and benefits of hashing include the following:

1) Data integrity. Hashing is commonly used to ensure data integrity. By generating a hash value for an amount of data, such as a file or message, a user can later compare it with the hash value of the received data to verify if any changes or corruption occurred during transmission.

2) Efficient data retrieval. Hashing enables efficient data retrieval in hash tables, especially when dealing with large data sets. It uses functions or algorithms to map object data to a representative integer value. A hash can then be used to narrow down searches when locating these items on that object data map. For example, in hash tables, developers store data -- perhaps a customer record -- in the form of key and value pairs. The key identifies the data and operates as an input to the hashing function, while the hash code or the integer is then mapped to a fixed size. Typically functions supported by hash tables include insert (key, value), get (key) and delete (key).

3) Digital signatures. In addition to enabling rapid data retrieval, hashing helps encrypt and decrypt digital signatures used to authenticate message senders and receivers. In this scenario, a hash function transforms the digital signature before both the hashed value -- known as a message digest -- and the signature are sent in separate transmissions to the receiver. Upon receipt, the same hash function derives the message digest from the signature, which is then compared with the transmitted message digest to ensure both are the same. In a one-way hashing operation, the hash function indexes the original value or key and enables access to data associated with a specific value or key that's retrieved.

4) Password storage. Hashing is widely used for secure password storage. Instead of storing passwords in plain text, they're hashed and stored as hash values. This adds an extra layer of security so even if the hash values are compromised, it's computationally infeasible to reverse-engineer the original passwords.

5) Fast searching. Hashing algorithms are designed to organize data into easily searchable buckets. This makes searching for specific data faster compared to other data structures. Hashing is particularly useful in applications that require rapid search results, such as databases and search engines.

6) Efficient caching. Hash tables are commonly used to configure caching systems. By using hash values as keys, data can be quickly retrieved from cache memory, reducing the need to access slower storage systems. This improves overall system performance and response times.

7) Space efficiency. Hashing enables efficient use of storage space. Hash values are typically shorter than the original data, making them more  compact and easier to store. This is especially beneficial when dealing with large data sets or limited storage resources.

8) Cryptographic applications. Hashing plays a crucial role in various cryptographic algorithms. Cryptographic hash functions are used to generate digital signatures, authenticate messages and ensure data integrity and authenticity. Hashing algorithms such as Secure Hash Algorithm 2, or SH-2, are widely used in cryptographic applications.

9) Data compression. By employing coding algorithms such as the Huffman coding algorithm, which is a lossless compression algorithm, hashing can be used to encode data efficiently.

10) Database management. When dealing with large data sets, combing through multiple entries to obtain the necessary data can be intimidating. Hashing offers an alternative by letting users search for data records using a search key and a hash function rather than an index structure. Hash files organize data into buckets, each of which can hold numerous records. The basic role of hash functions is to map search keys to the exact location of a record within a given bucket.


[Return back to answer.md](/answer.md)