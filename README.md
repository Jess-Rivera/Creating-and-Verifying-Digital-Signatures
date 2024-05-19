# Creating and Verifying Digital Signatures

## Objective
The goal of this lab is to create and verify a digital signature as well as create and approve a certificate signing request.


### Skills Learned

- Create a Private and Public Key Pair
- Create a Digital Signature
- Receive and Verify a Digital Signature
- Observe a Signature Verification Failure
- Create a Certificate Signing Request (CSR)
- Receive and Approve a CSR as a Certificate Authority (CA)


### Tools Used
<div>
    <img src="https://img.shields.io/badge/-Alma%20Linux-082336?&style=for-the-badge&logo=AlmaLinux&logoColor=white" />
    <img src="https://img.shields.io/badge/-OpenSSL-999999?&style=for-the-badge&logo=OpenSSL&logoColor=white&logoWidth=40&logoSize=auto"/>
</div>


## Steps

### Private and Public Key Pairs, Verifying Signature

In this portion of the lab, I created directories of two fictitous people, Alice and Bob. Then, as Alice, I created a private key, 
```
openssl genpkey -algorithm RSA -out alice_privatekey.pem
```
<details>
    <summary>Screen: Creating a Private Key</summary>
        <img src="https://github.com/Jess-Rivera/Creating-and-Verifying-Digital-Signatures/blob/main/CRYPTOGRAPHY_create_private_key.PNG" />
</details>
Breaking down the command, `openssl` is the encrption toolkit being used. `genpkey` is the command to create a private key. The `-algorithm RSA` designates to use RSA key algorithm and `-out` specifies the output file name, PEM being the default format for openssl private encryption key.

Then a public key is created, 
```
openssl rsa -in alice_privatekey.pem -out alice_publickey.pem -pubout -outform PEM
```
<details>
    <summary>Screen: Creating a Public Key</summary>
        <img src="https://github.com/Jess-Rivera/Creating-and-Verifying-Digital-Signatures/blob/main/CRYPTOGRAPHY_create_public_key.PNG" />
</details>

Again, `openssl` is the encrption toolkit being used. `rsa` specifies the function to process RSA keys. `-in alice_privatekey.pem` specifices the input file to be read from, and `-out alice_publickey.pem -pubout -outform PEM` specifies the output file name, specifies it is a pubclic key vice the default private key output, and specifies the PEM format.  

Next creating the message Alice wants to send Bob,
```
openssl dgst -sha256 -sign alice_privatekey.pem -out alice_signature.bin alice_digest.txt
```
<details>
    <summary>Screen: Creating a Hash from Digest</summary>
        <img src="https://github.com/Jess-Rivera/Creating-and-Verifying-Digital-Signatures/blob/main/CRYPTOGRAPHY_create_hash_from_digest.PNG" />
</details>

Again using `openssl`, this time with the `dgst` function. `sha-256` specifies the hash function, although sha-256 is the default. `-sign alice_privatekey.pem` is signing the digest using the private key, `-out alice_signature.bin` defines the output file to a binary format, and finally the file being hashed `alice_digest.txt`.

Then, Alice can send Bob the message, the hash, and the publickey. Bob can then verify the authenticity of the message,
```
openssl dgst -sha256 -verify alice_publickey.pem -signature alice_signature.bin alice_digest.txt
```
<details>
    <summary>Screen: Sending Message and Verifying Original and Authentic</summary>
        <img src="https://github.com/Jess-Rivera/Creating-and-Verifying-Digital-Signatures/blob/main/CRYPTOGRAPHY_send_verify_digest.PNG" />
</details>

Still using `openssl` with the `dgst` function. `sha-256` specifies the hash function, this time with the `-verify` command specifying the public key file, the signature file and finally the file to be read. The output certifies that the file is infact original and from Alice. I changed the message file and verified that the output would verify it to no longer be original.
<details>
    <summary>Screen: Verifying a non-Original</summary>
        <img src="https://github.com/Jess-Rivera/Creating-and-Verifying-Digital-Signatures/blob/main/CRYPTOGRAPHY_change_dgst_verify.PNG" />
</details>





## Challenges and Solutions


## Reflection and Learning
