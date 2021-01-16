# Symmetric Encryption

Symmetric encryption is a two-way process that uses the same key for encryption and decryption.

Extremely secure

Relatively fast

Problems: Must share the key

Good for encryting your own data

## Data Encryption Standard (DES)

Developed in the early 1970's by IBM

Encrypted using a 56 bit symmetric key

Key is typically 64 bits, but only the first 56 bits are used

The cipher text is the same size as the plain text

## Triple DES

Apply DES three times with two or three different keys

## AES (Advanced Encryption Standard)

Uses 128, 192 or 256 bit keys

Fixed block size of 128 bits

10 rounds for 128 bit keys
12 rounds for 192 bit keys
14 rounds for 256 bit keys

| Key Size | Time to Crack                |
|----------|------------------------------|
|  56 bit  | 399 seconds                  |
| 128 bit  | 1.02 x 10<sup>18</sup> years |
| 192 bit  | 1.87 x 10<sup>37</sup> years |
| 256 bit  | 3.31 x 10<sup>56</sup> years |

Encrypt in block units, not a single byte at a time

### Class Properties

CipherMode Mode

Default value is Cipher Block Chainging

In most cases, do not change the default mode

PaddingMode Padding 

Describes how to handle plaintext that is not divisible into exactly 128 bit chunks

ANSI X923: End padded with zeros and the final byte contains the length of the padding

ISO 10126: End padded with random data and the final byte contains the number of bytes that were padded

None: No padding

PKCS7: Fill each byte that needs to be padded with the number of bytes that needed to be padded.  For example, if four bytes need to be padded, each of the four bytes will be filled with the value four.

Zeros:  Each byte that needs to be padded is filled with a zero

Default padding in .NET is PKCS7

byte[] Key:

Generate with RNGCryptoServiceProvider or call the GenerateKey() method, which uses RNGCryptoServiceProvider to supply the key

Initialization Vector (IV) is a byte array

Also called nonce or number once

IV does not need to be kept secret

## Symmetric Encryption Libraries in .NET

`namespace System.Security.Cryptography`

`SymmetricAlgorithm` abstract base class

`AESCryptoServiceProvider` class

## Symmetric Encryption Pattern in .Net

Create encryption key, a random number stored in an array of bytes, by using the `RNGCrytpoServiceProvider` class' `GetBytes()` method.

Instantiate `AesCryptoServiceProvider` class

Set `Mode` property

Set `Padding` property

Set `Key` property

Set `IV` (Initialization Vectory) property

Instantiate `Stream`

Get encryptor by calling `CreateEncrypter()` method on `AesCryptoServiceProvider` object

Instantiate `CryptoStream` object by passing the `Stream`, Encryptor, and `CryptoStreamMode`.

Call the `Write()` method on the `CyrptoStream` object to write the encrypted data to the `Stream`.

Call the `FlushFinalBlock()` method on the `CryptoStream` object. 

Call the `ToArray()` method on the `Stream` object to get an array of bytes that contain the encrypted data.

To decrypt the data, use the same pattern described above to encrypt data, but call the `CreateDecryptor()` method of the `AesCryptoServiceProvider`