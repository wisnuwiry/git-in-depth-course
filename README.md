# Summary Git in Depth Course

## About Git

**What is Git?**

Distributed Version Control System

**How Does Git Store Information?**

- Git save information like a key and value store
- The `Value = Data`
- The `Key = Hash of the Data`
- You can use the key to retrive the content

The `key` is SHA1 and it includes:

- Is a cryptographic hash function
- Given a price of data, it produces a 40 digits hexa number
- Value should always be the same if the given input the same