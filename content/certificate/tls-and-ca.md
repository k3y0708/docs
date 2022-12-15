+++
categories = ['Development', 'VIM']
date = '2022-11-03'
description = 'Create TLS certificates with a CA using OpenSSL'
tags = ['TLS', 'SSL', 'CA', 'Certificate', 'OpenSSL', 'Security']
title = 'CA and TLS'
lastModifierDisplayName = "Keyvan Atashfaraz"
lastModifierEmail = "keyvan@atashfaraz.de"
+++

This is a guide to create a TLS certificate with a CA using OpenSSL.

## Generate TLS Certificate

**Required software**:
- OpenSSL

**Steps:**
1. Generate Private keys
2. Generate CA
3. Generate Certificate

## Private keys
We are creating private keys with an eliptic curve
```bash
# For the ca
openssl ecparam -name brainpoolP512t1 -out brainpoolP512t1-ca.pem
openssl ecparam -in brainpoolP512t1.pem -genkey -noout -out ca-pkey.pem

# For the cert
openssl ecparam -name brainpoolP512t1 -out brainpoolP512t1-tls.pem
openssl ecparam -in brainpoolP512t1.pem -genkey -noout -out tls-pkey.pem
```

### CA

With the private key you are able to generate a ca file. The flag `-days` is the expiry time.
```bash
openssl req -x509 -new -nodes \
    -key ca-pkey.pem -sha256 \
    -days 365 -out ca.crt
```

### Certificate

We are also able to create a signing request for a tls certificate
