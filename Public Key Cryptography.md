# Public Key Cryptography: RSA Keys

Undoubtedly, each of us has created an RSA key pair at least once in our lives, for example, to connect to GitHub without entering a password each time. Usually, the entire process takes a couple of minutes — just follow a simple instruction.

But what exactly happens when you generate these keys?

Do you know what is contained in the `~/.ssh/id_rsa` file? Why does SSH create two files, both in different formats? Have you noticed that one file starts with the characters "ssh-rsa," and the other starts with `-----BEGIN RSA PRIVATE KEY-----`? Have you paid attention to the fact that sometimes the header of the second file says "BEGIN PRIVATE KEY" instead of "RSA"?

I believe every developer should have at least a minimal understanding of the various formats of RSA keys. This becomes even more important if you want to pursue a career in infrastructure management.

## RSA Algorithm

Since the invention of public-key cryptography, various key pair generation systems have been devised. One of the first is RSA, created by three brilliant cryptographers and dating back to 1977. The history of RSA is interesting in that the scheme was initially invented by English mathematician Clifford Cox. However, he was required to keep it secret at the request of the British intelligence, for which he worked.

It's worth noting that RSA is not synonymous with public-key cryptography; it's just one of the possible implementations. RSA, still more than 40 years after its publication, remains one of the most common algorithms. In particular, it is the standard algorithm used for generating SSH key pairs. Given that today every developer has their public key added to GitHub, BitBucket, and similar systems, it can be confidently said that RSA is quite widely used.

In this article, I won't delve into the internal workings of the RSA algorithm. If you're interested in the details of this algorithm, its mathematical foundation, you can find plenty of sources both on the Internet and in textbooks. The theory behind RSA is far from trivial, but it's definitely worth spending time on if you want to seriously delve into the mathematical aspect of cryptography.

In this article, I will explore two methods of creating RSA key pairs and the formats for storing them. Applied cryptography, like many other topics in computer science, is constantly evolving, and tools often change. Finding out how to perform a specific action is usually not difficult (thanks to StackOverflow), but what can be challenging is getting a clear understanding of what is happening behind the scenes.

In all the examples provided in this article, a 2048-bit RSA key is used, created specifically for this purpose. Therefore, all symbols you see are taken from a real example. After writing the article, this key was destroyed.

## PEM Format

Let's start the conversation about key pairs with the format of their storage. Currently, the most common storage format is PEM (Privacy-enhanced Electronic Mail). As the name suggests, this format was originally created for encrypting email but later became a general format for storing cryptographic data such as keys and certificates. It is described in RFC 7468 ("Textual Encodings of PKIX, PKCS, and CMS Structures").

An example of a private key in PEM format looks like this:

```
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQCy9f0/nwkXESzk
L4v4ftZ24VJYvkQ/Nt6vsLab3iSWtJXqrRsBythCcbAU6W95OGxjbTSFFtp0poqM
cPuogocMR7QhjY9JGG3fcnJ7nYDCGRHD4zfG5Af/tHwvJ2ew0WTYoemvlfZIG/jZ
7fsuOQSyUpJoxGAlb6/QpnfSmJjxCx0VEoppWDn8CO3VhOgzVhWx0dcne+ZcUy3K
kt3HBQN0hosRfqkVSRTvkpK4RD8TaW5PrVDe1r2Q5ab37TO+Ls4xxt16QlPubNxW
eH3dHVzXdmFAItuH0DuyLyMoW1oxZ6+NrKu+pAAERxM303gejFzKDqXid5m1EOTv
k4xhyqYNAgMBAAECggEBALJCVQAKagOQGCczNTlRHk9MIbpDy7cr8KUQYNThcZCs
UKhxxXUDmGaW1838uA0HJu/i1226Vd/cBCXgZMx1OBADXGoPl6o3qznnxiFbweWV
Ex0MN4LloRITtZ9CoQZ/jPQ8U4mS1r79HeP2KTzhjswRc8Tn1t1zYq1zI+eiGLX/
sPJF63ljJ8yHST7dE0I07V87FKTE2SN0WX9kptPLLBDwzS1X6Z9YyNKPIEnRQzzE
vWdwF60b3RyDz7j7foyP3PC0+3fee4KFdJzt+/1oePf3kwBz8PQq3cuoOF1+0Fzf
yqKiunV2AXI6liAf7MwuZcZeFPZfHTTW7N/j+FQBgAECgYEA4dFjib9u/3rkT2Vx
Bu2ByBpItfs1b4PdSiKehlS9wDZxa72dRt/RSYEyVFBUlYrKXP2nCdl8yMap6SA9
Bfe51F5oWhml9YJn/LF/z1ArMs/tuUyupY7l9j66XzPQmUbIZSEyNEQQ09ZYdIvK
4lbySJbCqa2TQNPIOSZS2o7XNG0CgYEAyuFVybOkVGtfw89MyA1TnVMcQGusXtgo
GOl3tJb59hTO+xF547+/qyK8p/iOu4ybEyeucBEyQt/whmNwtsdngtvVDb4f7psz
Frmqx7q7fPoKnvJsPJds9i2o9B7+BlRY3HwcvKePsctP96pQ0RbOFkCVak6J6t9S
k/qhOiNJ9CECgYEAvDuTMk5tku54g6o2ZiTyir9GHtOwviz3+AUViTn4FdIMB1g+
UsbcqN3V+ywe5ayUdKFHbNFqz92x4k7qLyBJObocWAaLLTQvxBadSE02RRvHuC8w
YXbVP8cYCaWiWzICdzINrD2UnVBN2ZBxZOw+970btN6oIWCnxOOqKt7oip0CgYAp
Fekhp9enoPcL2HdcLBa6zZHzGdsWef/ky6MKV2jXhO9FuQxOKw7NwYMjIRsGsDrX
bjnNSC49jMxQ6uJwoYE85vgGiHI/B/8YoxEK0a4WaSytc7qnqqLOWADXL0+SSJKW
VCwdqHFZOCtBpKQpM80YhIu9s5oKjp9SiHcOJwdbAQKBgDq047hBqyNFFb8KjS5A
+26VOJcC2DRHTprYSRJNxsHTQnONTnUQJl32t0TrqkqIp5lTRr7vBH2wJM6LKk45
I7BWY4mUirC7sDGHl3DaFPRBiut1rpg0kSKi2VNRF7Bb75OKEhGjvm6IKVe8Kl8d
5cpQwm9C7go4OiorY0DVLho2
-----END PRIVATE KEY-----
```

In principle, dealing with the PEM format can be inferred from the standard headers and footers that identify the content. The presence of dashes and the two words `BEGIN` and `END` is consistent, while the part `PRIVATE KEY` changes depending on the content, including cases where the PEM file contains something other than a key, such as an X.509 certificate for SSL.

The PEM format specifies that the content body (the part between the header and footer) is encoded using Base64.

If the private key was encrypted with a password, the header and footer would differ.

```
-----BEGIN ENCRYPTED PRIVATE KEY-----
MIIFHzBJBgkqhkiG9w0BBQ0wPDAbBgkqhkiG9w0BBQwwDgQIf75rXIakuSICAggA
MB0GCWCGSAFlAwQBKgQQf8HMdJ9FZJjwHkMQjkNA3gSCBNClWB7cJ5f8ThrQtmoA
t2WQCvEWTY9nRYwaTnL1SmXyuMDFrX5CWEuVFh/Zj77KB9jhBJaHw2XtFXxF8bV7
F10u93ih/n0S5QwN9CSPDhRp2kD5lIWB8WVG+VgtncqDrAfJRmpuPmzpjMJBxE2r
MvWJG5beMCS25qD0mAxihtbriqFoCtEygQ7vsSfeQpaBQvT5pKLOVaVgwFTFTf+7
cgqB8/UKKmPXSM4GMJ9VNAvUx0mAxI9MnUFlBWimK76OAzdlO9Si99R8OiRRS10x
AO1AwWSDHGWpbckK0g9K7wLgAgOw8LLVUJh67o9Mfg58DP9Ca0ZdPPVo0C7oavBD
NFlUsKqmSfqfgOAm4qGJ7GB3KgWGFdz+yexNLRLN63hE6qACAuQ1oLmwoorE8toh
MhT3c6IxnVWlYNXJkkb5iV9e8E2X/xzibvwv+CJJ9ulCU8uS7gp0rjlCKFwt/8d4
g3Cef/JWn9nI9YwRLNShJeQOe8hZkkLXHefUhBa2o2++C5C6mgWvuYLK6a0zfCMY
WCqjKKvDQfuxwDbeM03jJ97Je6dXy7rtJvJd10vYvpIVtHnNSdg1evpSiaAmWt4C
X5/AzbHNvwTIEvILfOtYvxLB/RdWqr1/VXuH4dJF6AYtHfQHjXetmL/fDA86Bqf6
Eb+uDr+PPuH4qw1tfJBdTSOOJzhhPqdT4ERYnOvfNxTKzsKYZT+kWvWXe9zyO13W
C0eceVi4rBjKpKpKecKDgFJGZ1u7jS0OW3FDIOfm/osu9z25g5CVIpuWU3JquWib
GatHET9wIEg7LRqC/i65q6tCnd9azevKtiur1I0tuh05iwP5kZ8drIzaGdObuvK1
/pbEPnj1ZcRlAZ34jnG841xvf4vofrOE+hGTNF5HypOCvO/8Lms3aB6NletIvHBE
99ynQyF9TAgSAFAumOws+qnRcnfVOF5lzIEE2pmeMVMqi5s7TT4hlhOuCbyfEFU8
xOXxNazT+0o7urIYOc77vA1LsWrk+9dAfm43CbBZvYav/gMoBc5fsLgAUAm1lkt5
5Hjaf+iMIN0v7aEKDrNDOtyQr13YdyuEClzXxeMtlhU+QfErpQHvH0jE4gywEgz7
tvVGwrbiLgg0y537+kg0/rS3N0eI94GhY0q/nR/QFObbN0nmoIYVVSGtufJx1r9v
YEVZA7HZE9pjnun1ylE1/SoYc/816rjBUcW5CCbkMDIz1LsFPr2SkQeHTNzK3/9J
Kny1lerfA+TA/hUyZ1KJjxuao+rJkH2fJ25qs3r6NP+PPbq3sAl1TPGhMCnNaFdo
YQWDDwz26ZR2ywfsquqLXMwnIEeUI/hQTng9ZxLkJMY22rQSA9nsdvR8S1b0U8Qu
ViYEjCTMWF8HEFFO721MlkTgchzq6fiF+9ZydCpVUJWolcfw1OgUvvTSI7Eyhelb
7fc1fTVFeEMsHrtjpu8dg+IaCNraBzv5QZx6MYW7SSoTVp8mJoPnzYbsZs9nHJGX
iQOFmO/sIryOoeJlpOCGT55yU74yRXrBsYZyLz0P9K1FDQS6l9W33BqmF9vSXujs
kSByq8v1OU0IqidnMmZtTDSRlpQL/oadqQnsA6jiWyMznuUEU8tfgUALE4DKRq8P
wBLKVfMiwcWAbl121M2DCLj9/g==
-----END ENCRYPTED PRIVATE KEY-----
```

When using the PEM format to store cryptographic keys, the content body follows a format called PKCS #8. Initially, this standard was created by a private company (RSA Laboratories) but later became a de facto standard and was described in various RFCs, including RFC 5208 ("Public-Key Cryptography Standards (PKCS) #8: Private-Key Information Syntax Specification Version 1.2").

To describe the structure of the data, the PKCS #8 format uses the ASN.1 (Abstract Syntax Notation One) language, while DER (Distinguished Encoding Rules) is used to serialize this structure into binary format. This means that Base64 decoding of the content will result in some binary value that can only be processed by an ASN.1 parser.

Let's summarize the above information in the form of a diagram:

```
-----BEGIN label-----
+--------------------------- Base64 ---------------------------+
|                                                              |
| PKCS #8 content:                                             |
| ASN.1 language serialised with DER                           |
|                                                              |
+--------------------------------------------------------------+
-----END label-----
```
Please note that due to the peculiarities of the ASN.1 structure, the bodies of PEM-formatted keys always start with the same characters: `MIG` for 1024-bit keys, and `MII` for 2048 and 4096-bit keys.
