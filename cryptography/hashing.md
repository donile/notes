# Hashing

## What is hashing?

Message: Data to be encrypted

Message Digest: Encrypted data

## Hashing Function Properties

Easy to compute hash value

Infeasible to compute message from hash value

Infeasible to modify message without modifying hash

Infeasible for two different message to produce the same hash

## Hashing Algorithms

MD5

SHA-1

SHA-256

SHA-512

## MD5

Produces 128 bit (16 bytes) has value

Algorithm contains hash collision problems (two different messages can produce the same hash)

## `HashAlgorithm` class

The base class for all hashing algorithms in .NET

To create a hash:

Instantiate a class that descends from the abstract class `HashAlgorithm` by calling the `Create()` method on the class.

Call the `ComputeHash(byte[] toBeHashed)` method on the `HashAlgorithm` object, passing in the bytes to be hashed.  This method will return the hash as an array of bytes.

## Hashed Message Authentication Code (HMAC)

Supply cryptographic hashing function with a secret key.

Used to verify the integrity of a message.

Only key holders can produce the hash value to verity integrity of message.

The cryptographic strength of a HMAC depends on the size of the key that is used.

The most common attack on an HMAC is a brute force attack.

HMAC substantially reduce the potential for a collision because the addition of the secret key adds entropy to the hashing algorithm.

## `KeyedHashAlgorithm` Class

Abstract base class from which all HMAC producing classes inherit.

Generate key of random numbers and store as an array of bytes (as shown in Cryptographic Random Numbers).

Instantiate an instance of the HMAC class that will be used to create the HMAC, passing in the key as an argument to the constructor.

`var hmac = new HMACSHA512(key);`

Use `Encoding.UTF8.GetBytes(string message)` to convert the string to an array of bytes.

Pass the array of bytes that contains the message to the `ComputeHash()` method of the HMAC object.  It will return the hash.
