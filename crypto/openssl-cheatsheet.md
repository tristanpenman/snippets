# OpenSSL Cheatsheet

## Useful Commands

#### Print a certificate as text:

    openssl x509 -in example.pem -text

#### Extract the public key from a certificate:

    openssl x509 -pubkey -noout -in example.pem > example.pub

Alternatively, you can use `ssh-keygen`, e.g:

    ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub

### Signatures

#### Sign a document:

    openssl dgst -sha1 -sign myprivate.pem -out data.sig data.txt

#### Verify a signature:

This verifies a signature using a public key, like the one extracted earlier:

    openssl dgst -sha256 -verify example.pub -signature data.sig data.txt

### Conversion

#### Convert certificate from PEM format to DER format:

    openssl x509 -outform der -in example.pem -out example.der

#### And vice-versa:

    openssl x509 -inform der -in example.der -out example.pem
