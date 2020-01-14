# Secure Password Storage

## Salted Hashes

A **salt** is a series of random characters added to the end of a password before it is passed through the hashing function.

Adding a salt to a password makes rainbow and brute force attacks ineffective.

A unique salt should be added to each password.

The salt can be stored in the database.

## Password Based Key Derivation Functions

Accepts password, salt and number of iterations.  The hash value is repeatedly hashed the n number of iterations.

Good default value of iterations is 50,000

Double the default value of two years

## Using Password Based Key Derivation Functions in .NET

Instantiate `Rfc2898DeriveBytes` object by passting `password`, `salt` and `numberOfIterations` to constructor

Call `GetBytes()` to return the hash produced by the PBKDF