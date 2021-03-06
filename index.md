**Answer:**

# Why does storing passwords as plaintext format in databases pose a threat, and how can we mitigate this risk by using cryptographic hash functions?

### Risks of storing raw passwords
Databases, which are storage devices similar to hard drives in your computer, are typically used to store passwords. Administrators are the people who have the highest access rights in a system, and this privilege allows them to retrieve these plaintext passwords from databases easily. As ordinary people tend to reuse the same passwords in multiple places such as emails or social networks, administrators with maleficent intent can access users' accounts in other systems.  Similarly, it is not uncommon for websites to have a security breach, which inadvertently leads to users' passwords being exposed to outside people.

### Solutions

One viable solution is converting raw passwords into strings that satisfy these criteria:
- They yield no meaning, and it is incredibly challenging for outsiders to guess the patterns
- The final string is utterly impossible for people to try tracing back the original password. After all, our ultimate goal is still trying to protect the original password from being disclosed.

These strings will be stored in databases instead. Every time a user types in his password, it will be converted again to this meaningless string. And a match between this string and the one saved in the system results in access granted to this user. In other words, we still can verify that this user is a genuine one, as he can provide his password correctly, without the need to keep the plaintext password in our database.

### What are cryptographic hash functions?

Converting a string of arbitrary length(raw password) into a fixed-size one (a meaningless string in the database) can be done using a hash function. Both the input and output pair is called a hash.

More specifically, a hash function works like a fruit blender, of which you may put in several fruits and expect the juice to come out. Each run's product will differ based on what kinds of fruits were put in, but it is infeasible to guess which fruits were used.

In cryptography, we use a special type of hash function called a cryptographic hash function. Typically, this cryptographic hash function should possess the below characteristics:
- It is deterministic: It yields the same output if the same input is given. You would not want to be in a situation where you cannot log in to your account even with the correct password.
- It is fast in calculating the hashed value: Users may be frustrated if they have to wait for minutes to log in to their account.
- It is a one-way conversion: Others cannot use the output to recalculate the input effortlessly, as this is the sole purpose why a hash function is in use.
- Each output is unique: You do not want another person to type in a different password and still can access your account by chance.
- Small change in the input leads to a significant change in the output: This is to prevent the hackers from guessing the pattern in passwords generated and try to abuse the system.

### What are the popular cryptographic hash functions?

MD5 is one of the most commonly used hash functions to store sensitive information, such as passwords, credit card numbers and social security numbers, in MySQL databases. However, we will not go into details about the mechanism behind this hash function. Nevertheless, it ensures all the above requirements are satisfied.

| Input      |             Output               |
| -----------|----------------------------------|
| mr**b**ean | f1a991821c018290bd4f2ab128f14eb7 |
| mr**d**ean | 89ab112ac014ad65c829c83cec03ddbd |
| mr**l**ean | a1abf4f8af1c88c5d18f539b0768328a |

Above are the hashed values of some inputs. All of them have a length of 32 characters made from a list of numeric values (1-9) and alphabetical values (a-f). "mrbean" is the only input that can generate "f1a991821c018290bd4f2ab128f14eb7" through the MD5 hashing function. You keep having the same "f1a991821c018290bd4f2ab128f14eb7" output every time you run this function with "mrbean" as the input. It is almost impossible to deduce "mrbean" from this output. Finally, even a small change in the third character of the input leads to a big change in the output.

### Drawbacks of the cryptographic hash function

Nevertheless, cryptographic hash functions are not a panacea for saving sensitive information securely in the database. MD5 method mentioned above also has its weaknesses, and one way to break it is using the brute force method. Brute force means trying all the possible combinations to achieve the desired result. In MD5, a hacker can create a list of all possible inputs and run them through the MD5 function to get the expected output. He only needs to compare the result with the one stored in the database and retrieves the original password which creates the result that matches. MD5's strength of computing the hashed values extremely fast further exacerbates the problem and makes it more vulnerable to the brute force approach.

### Final remarks

In conclusion, it is not a good idea to store sensitive information in databases in a plaintext format as it can fall into the wrong hands. Instead, administrators need to convert these pieces of information into fixed-length meaningless strings before saving them in databases using cryptographic hash functions. Unfortunately, they are still prone to security attacks, with brute force being one of them.
