# Cryptographic Random Numbers

## `System.Random` Class
Generates psuedo random numbers

If the same seed value is provided, the same random numbers will be generated.

## `RNGCryptoServiceProvider` Class

`GetBytes` method accepts an array of bytes, populates the array with random data and returns the array.

If a 256 bit random number is required, provide `GetBytes` method with an array of 32 bytes.  Since there are 8 bits per byte, there will be 256 random bits in the returned byte array.