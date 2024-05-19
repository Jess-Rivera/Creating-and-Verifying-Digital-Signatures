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

### Creating and Approving a Certificate Signing Request

In this portion, I created a certificate signing request (CSR) and then sent it to a Certificate Authority(CA) for approval (recieve a crt). I acted as both parties. 

First, I created the CSR as SecPlusLLC,
```
openssl req -newkey rsa:2048 -keyout SecPlusLLC_privatekey.pem -out SecPlusLLC.csr
```
<details>
    <summary>Screen: Creating the CSR</summary>
        <img src="https://github.com/Jess-Rivera/Creating-and-Verifying-Digital-Signatures/blob/main/CRYPTOGRAPHY_create_CSR.PNG" />
</details>

`openssl req` establishes the toolkit and a certificate request. `-newkey rsa:2048` generates the new request using RSA with 2048 bits. `-keyout SecPlusLLC_privatekey.pem` specifices output file for the new private key, and `-out SecPlusLLC.csr` writes the certificate request to the specified file.

The CSR was then sent to the CA, CertAuth,  for approval. As the CA, I created my own private key in a similar manner to creating Alice's private key in the first portion: `openssl genpkey -algorithm RSA -out CA_privatekey.pem`. Finally, the CA signs the certificate,
```
openssl req -newkey rsa:2048 -nodes -keyout CA_privatekey.pem -x509 -days 1999 -out SecPlusLLC.crt
```
<details>
    <summary>Screen: CA signing the CSR to create the CRT</summary>
        <img src="https://github.com/Jess-Rivera/Creating-and-Verifying-Digital-Signatures/blob/main/CRYPTOGRAPHY_create_privatekey_and_crt.PNG" />
</details>

`openssl req -newkey rsa:2048` to create the new crt similar as creating the csr. However, the `-nodes` specifies the private key is not encrypted. `-x509 -days 1999` specifies that the CA is creating a signed certificate instead of a request and specifies the number of days the certificate is valid (default of 30 days). Finally, specifying the output with `-out SecPlusLLC.crt`. This was then sent to SecPlusLLC.

<details>
    <summary>Screen: Receiving the CRT</summary>
        <img src="https://github.com/Jess-Rivera/Creating-and-Verifying-Digital-Signatures/blob/main/CRYPTOGRAPHY_verify_crt_received.PNG" />
</details>



## Challenges and Solutions
The most difficult part of this lab was learning all the arguments required for the `openssl` toolkit. Thankfully, it was as easy as pulling up the OpenSSL.org site and referencing the docs. 

## Reflection and Learning
While I don't understand the underlying functionality of how OpenSSL works, using it as a tool to create signatures and verify origin and authenticity were pretty straightfoward. It bugs me in the back of my head to use a tool and not understand the underlying process like I would normally do for my job in the Nuclear Field. But in a nuclear plant, there is only a limited number of systems and they rarely change so it is feasible with enough time and practice to know every system broken down to the simplest part. In the technology field, new tools are created so rapidly and they have so many functions. I'm sure with enough time I could become an extremely proficient user in `openssl` like I would with a system in a plant, but to expect myself to be very proficient with the amount of tools available to me is an unrealistic expectation. 
The second portion of the lab did make sense to me for creating a request and getting the approval back. But I am still wondering how to then verify the crt.
Upon conducting research, I could use the `openssl verify` function and pass it the SecPlusLLC.crt file, `-CAfile SecPlusLLC.crt` and since CertAuth is the Trusted Authority, use `-trusted CA_cert.crt`. While I did not create it in this lab, a Trusted Authority would have created this and it would be in the Client CA ROOT list. Fantastic!
