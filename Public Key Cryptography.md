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

## OpenSSL and ASN.1
OpenSSL can directly decode a key in PEM format and display its underlying ASN.1 structure using the `asn1parse` module.
```
$ openssl asn1parse -inform pem -in private.pem
    0:d=0  hl=4 l=1214 cons: SEQUENCE          
    4:d=1  hl=2 l=   1 prim: INTEGER           :00
    7:d=1  hl=2 l=  13 cons: SEQUENCE          
    9:d=2  hl=2 l=   9 prim: OBJECT            :rsaEncryption
   20:d=2  hl=2 l=   0 prim: NULL              
   22:d=1  hl=4 l=1192 prim: OCTET STRING      [HEX DUMP]:308204A40201000282010100B2F5FD3F9F0917112
   CE42F8BF87ED676E15258BE443F36DEAFB0B69BDE2496B495EAAD1B01CAD84271B014E96F79386C636D348516DA74A68
   A8C70FBA882870C47B4218D8F49186DDF72727B9D80C21911C3E337C6E407FFB47C2F2767B0D164D8A1E9AF95F6481BF
   8D9EDFB2E3904B2529268C460256FAFD0A677D29898F10B1D15128A695839FC08EDD584E8335615B1D1D7277BE65C532
   DCA92DDC7050374868B117EA9154914EF9292B8443F13696E4FAD50DED6BD90E5A6F7ED33BE2ECE31C6DD7A4253EE6CD
   C56787DDD1D5CD776614022DB87D03BB22F23285B5A3167AF8DACABBEA40004471337D3781E8C5CCA0EA5E27799B510E
   4EF938C61CAA60D02030100010282010100B24255000A6A03901827333539511E4F4C21BA43CBB72BF0A51060D4E1719
   0AC50A871C57503986696D7CDFCB80D0726EFE2D76DBA55DFDC0425E064CC753810035C6A0F97AA37AB39E7C6215BC1E
   595131D0C3782E5A11213B59F42A1067F8CF43C538992D6BEFD1DE3F6293CE18ECC1173C4E7D6DD7362AD7323E7A218B
   5FFB0F245EB796327CC87493EDD134234ED5F3B14A4C4D92374597F64A6D3CB2C10F0CD2D57E99F58C8D28F2049D1433
   CC4BD677017AD1BDD1C83CFB8FB7E8C8FDCF0B4FB77DE7B8285749CEDFBFD6878F7F7930073F0F42ADDCBA8385D7ED05
   CDFCAA2A2BA757601723A96201FECCC2E65C65E14F65F1D34D6ECDFE3F85401800102818100E1D16389BF6EFF7AE44F6
   57106ED81C81A48B5FB356F83DD4A229E8654BDC036716BBD9D46DFD1498132545054958ACA5CFDA709D97CC8C6A9E92
   03D05F7B9D45E685A19A5F58267FCB17FCF502B32CFEDB94CAEA58EE5F63EBA5F33D09946C8652132344410D3D658748
   BCAE256F24896C2A9AD9340D3C8392652DA8ED7346D02818100CAE155C9B3A4546B5FC3CF4CC80D539D531C406BAC5ED
   82818E977B496F9F614CEFB1179E3BFBFAB22BCA7F88EBB8C9B1327AE70113242DFF0866370B6C76782DBD50DBE1FEE9
   B3316B9AAC7BABB7CFA0A9EF26C3C976CF62DA8F41EFE065458DC7C1CBCA78FB1CB4FF7AA50D116CE1640956A4E89EAD
   F5293FAA13A2349F42102818100BC3B93324E6D92EE7883AA366624F28ABF461ED3B0BE2CF7F805158939F815D20C075
   83E52C6DCA8DDD5FB2C1EE5AC9474A1476CD16ACFDDB1E24EEA2F204939BA1C58068B2D342FC4169D484D36451BC7B82
   F306176D53FC71809A5A25B320277320DAC3D949D504DD9907164EC3EF7BD1BB4DEA82160A7C4E3AA2ADEE88A9D02818
   02915E921A7D7A7A0F70BD8775C2C16BACD91F319DB1679FFE4CBA30A5768D784EF45B90C4E2B0ECDC18323211B06B03
   AD76E39CD482E3D8CCC50EAE270A1813CE6F80688723F07FF18A3110AD1AE16692CAD73BAA7AAA2CE5800D72F4F92489
   296542C1DA87159382B41A4A42933CD18848BBDB39A0A8E9F5288770E27075B010281803AB4E3B841AB234515BF0A8D2
   E40FB6E95389702D834474E9AD849124DC6C1D342738D4E7510265DF6B744EBAA4A88A7995346BEEF047DB024CE8B2A4
   E3923B0566389948AB0BBB031879770DA14F4418AEB75AE98349122A2D9535117B05BEF938A1211A3BE6E882957BC2A5
   F1DE5CA50C26F42EE0A383A2A2B6340D52E1A36
```
What you see in the provided fragment is the private key in ASN.1 format. Remember that DER is used only to transition from the textual representation of ASN.1 to binary data, so we won't see it until we decode the Base64 content into a file and open it in a binary editor.

Note that the ASN.1 structure contains a description of the object type (in this case, it is `rsaEncryption`).

The next step is to decode the `OCTET STRING` field, which is the actual key. Pay attention to the `-strparse parameter`, which specifies an offset equal to 22.

```
$ openssl asn1parse -inform pem -in private.pem -strparse 22
    0:d=0  hl=4 l=1188 cons: SEQUENCE          
    4:d=1  hl=2 l=   1 prim: INTEGER           :00
    7:d=1  hl=4 l= 257 prim: INTEGER           :B2F5FD3F9F0917112CE42F8BF87ED676E15258BE443F36DEAFB
    0B69BDE2496B495EAAD1B01CAD84271B014E96F79386C636D348516DA74A68A8C70FBA882870C47B4218D8F49186DDF
    72727B9D80C21911C3E337C6E407FFB47C2F2767B0D164D8A1E9AF95F6481BF8D9EDFB2E3904B2529268C460256FAFD
    0A677D29898F10B1D15128A695839FC08EDD584E8335615B1D1D7277BE65C532DCA92DDC7050374868B117EA9154914
    EF9292B8443F13696E4FAD50DED6BD90E5A6F7ED33BE2ECE31C6DD7A4253EE6CDC56787DDD1D5CD776614022DB87D03
    BB22F23285B5A3167AF8DACABBEA40004471337D3781E8C5CCA0EA5E27799B510E4EF938C61CAA60D
  268:d=1  hl=2 l=   3 prim: INTEGER           :010001
  273:d=1  hl=4 l= 257 prim: INTEGER           :B24255000A6A03901827333539511E4F4C21BA43CBB72BF0A51
    060D4E17190AC50A871C57503986696D7CDFCB80D0726EFE2D76DBA55DFDC0425E064CC753810035C6A0F97AA37AB39
    E7C6215BC1E595131D0C3782E5A11213B59F42A1067F8CF43C538992D6BEFD1DE3F6293CE18ECC1173C4E7D6DD7362A
    D7323E7A218B5FFB0F245EB796327CC87493EDD134234ED5F3B14A4C4D92374597F64A6D3CB2C10F0CD2D57E99F58C8
    D28F2049D1433CC4BD677017AD1BDD1C83CFB8FB7E8C8FDCF0B4FB77DE7B8285749CEDFBFD6878F7F7930073F0F42AD
    DCBA8385D7ED05CDFCAA2A2BA757601723A96201FECCC2E65C65E14F65F1D34D6ECDFE3F854018001
  534:d=1  hl=3 l= 129 prim: INTEGER           :E1D16389BF6EFF7AE44F657106ED81C81A48B5FB356F83DD4A2
    29E8654BDC036716BBD9D46DFD1498132545054958ACA5CFDA709D97CC8C6A9E9203D05F7B9D45E685A19A5F58267FC
    B17FCF502B32CFEDB94CAEA58EE5F63EBA5F33D09946C8652132344410D3D658748BCAE256F24896C2A9AD9340D3C83
    92652DA8ED7346D
  666:d=1  hl=3 l= 129 prim: INTEGER           :CAE155C9B3A4546B5FC3CF4CC80D539D531C406BAC5ED82818E
    977B496F9F614CEFB1179E3BFBFAB22BCA7F88EBB8C9B1327AE70113242DFF0866370B6C76782DBD50DBE1FEE9B3316
    B9AAC7BABB7CFA0A9EF26C3C976CF62DA8F41EFE065458DC7C1CBCA78FB1CB4FF7AA50D116CE1640956A4E89EADF529
    3FAA13A2349F421
  798:d=1  hl=3 l= 129 prim: INTEGER           :BC3B93324E6D92EE7883AA366624F28ABF461ED3B0BE2CF7F80
    5158939F815D20C07583E52C6DCA8DDD5FB2C1EE5AC9474A1476CD16ACFDDB1E24EEA2F204939BA1C58068B2D342FC4
    169D484D36451BC7B82F306176D53FC71809A5A25B320277320DAC3D949D504DD9907164EC3EF7BD1BB4DEA82160A7C
    4E3AA2ADEE88A9D
  930:d=1  hl=3 l= 128 prim: INTEGER           :2915E921A7D7A7A0F70BD8775C2C16BACD91F319DB1679FFE4C
    BA30A5768D784EF45B90C4E2B0ECDC18323211B06B03AD76E39CD482E3D8CCC50EAE270A1813CE6F80688723F07FF18
    A3110AD1AE16692CAD73BAA7AAA2CE5800D72F4F92489296542C1DA87159382B41A4A42933CD18848BBDB39A0A8E9F5
    288770E27075B01
1061:d=1  hl=3 l= 128 prim: INTEGER           :3AB4E3B841AB234515BF0A8D2E40FB6E95389702D834474E9AD8
    49124DC6C1D342738D4E7510265DF6B744EBAA4A88A7995346BEEF047DB024CE8B2A4E3923B0566389948AB0BBB0318
    79770DA14F4418AEB75AE98349122A2D9535117B05BEF938A1211A3BE6E882957BC2A5F1DE5CA50C26F42EE0A383A2A
    2B6340D52E1A36
```
In an RSA key, the fields represent specific components of the algorithm. In order, we can see:

- Modulus `n = pq`
- Public exponent `e`
- Private exponent `d`
- Two prime numbers `p` and `q`
- Values `d_p`, `d_q`, and `q_inv` (for efficiency using the Chinese Remainder Theorem)

If the key is encrypted, there are fields containing information about the cipher. However, the `OCTET STRING` field cannot be decoded because it is encrypted.

```
$ openssl asn1parse -inform pem -in private-enc.pem
    0:d=0  hl=4 l=1311 cons: SEQUENCE          
    4:d=1  hl=2 l=  73 cons: SEQUENCE          
    6:d=2  hl=2 l=   9 prim: OBJECT            :PBES2
   17:d=2  hl=2 l=  60 cons: SEQUENCE          
   19:d=3  hl=2 l=  27 cons: SEQUENCE          
   21:d=4  hl=2 l=   9 prim: OBJECT            :PBKDF2
   32:d=4  hl=2 l=  14 cons: SEQUENCE          
   34:d=5  hl=2 l=   8 prim: OCTET STRING      [HEX DUMP]:7FBE6B5C86A4B922
   44:d=5  hl=2 l=   2 prim: INTEGER           :0800
   48:d=3  hl=2 l=  29 cons: SEQUENCE          
   50:d=4  hl=2 l=   9 prim: OBJECT            :aes-256-cbc
   61:d=4  hl=2 l=  16 prim: OCTET STRING      [HEX DUMP]:7FC1CC749F456498F01E43108E4340DE
   79:d=1  hl=4 l=1232 prim: OCTET STRING      [HEX DUMP]:A5581EDC2797FC4E1AD0B66A00B765900AF1164D8
   F67458C1A4E72F54A65F2B8C0C5AD7E42584B95161FD98FBECA07D8E1049687C365ED157C45F1B57B175D2EF778A1FE7
   D12E50C0DF4248F0E1469DA40F9948581F16546F9582D9DCA83AC07C9466A6E3E6CE98CC241C44DAB32F5891B96DE302
   4B6E6A0F4980C6286D6EB8AA1680AD132810EEFB127DE42968142F4F9A4A2CE55A560C054C54DFFBB720A81F3F50A2A6
   3D748CE06309F55340BD4C74980C48F4C9D41650568A62BBE8E0337653BD4A2F7D47C3A24514B5D3100ED40C164831C6
   5A96DC90AD20F4AEF02E00203B0F0B2D550987AEE8F4C7E0E7C0CFF426B465D3CF568D02EE86AF043345954B0AAA649F
   A9F80E026E2A189EC60772A058615DCFEC9EC4D2D12CDEB7844EAA00202E435A0B9B0A28AC4F2DA213214F773A2319D5
   5A560D5C99246F9895F5EF04D97FF1CE26EFC2FF82249F6E94253CB92EE0A74AE3942285C2DFFC77883709E7FF2569FD
   9C8F58C112CD4A125E40E7BC8599242D71DE7D48416B6A36FBE0B90BA9A05AFB982CAE9AD337C2318582AA328ABC341F
   BB1C036DE334DE327DEC97BA757CBBAED26F25DD74BD8BE9215B479CD49D8357AFA5289A0265ADE025F9FC0CDB1CDBF0
   4C812F20B7CEB58BF12C1FD1756AABD7F557B87E1D245E8062D1DF4078D77AD98BFDF0C0F3A06A7FA11BFAE0EBF8F3EE
   1F8AB0D6D7C905D4D238E2738613EA753E044589CEBDF3714CACEC298653FA45AF5977BDCF23B5DD60B479C7958B8AC1
   8CAA4AA4A79C283805246675BBB8D2D0E5B714320E7E6FE8B2EF73DB9839095229B9653726AB9689B19AB47113F70204
   83B2D1A82FE2EB9ABAB429DDF5ACDEBCAB62BABD48D2DBA1D398B03F9919F1DAC8CDA19D39BBAF2B5FE96C43E78F565C
   465019DF88E71BCE35C6F7F8BE87EB384FA1193345E47CA9382BCEFFC2E6B37681E8D95EB48BC7044F7DCA743217D4C0
   81200502E98EC2CFAA9D17277D5385E65CC8104DA999E31532A8B9B3B4D3E219613AE09BC9F10553CC4E5F135ACD3FB4
   A3BBAB21839CEFBBC0D4BB16AE4FBD7407E6E3709B059BD86AFFE032805CE5FB0B8005009B5964B79E478DA7FE88C20D
   D2FEDA10A0EB3433ADC90AF5DD8772B840A5CD7C5E32D96153E41F12BA501EF1F48C4E20CB0120CFBB6F546C2B6E22E0
   834CB9DFBFA4834FEB4B7374788F781A1634ABF9D1FD014E6DB3749E6A086155521ADB9F271D6BF6F60455903B1D913D
   A639EE9F5CA5135FD2A1873FF35EAB8C151C5B90826E4303233D4BB053EBD929107874CDCCADFFF492A7CB595EADF03E
   4C0FE15326752898F1B9AA3EAC9907D9F276E6AB37AFA34FF8F3DBAB7B009754CF1A13029CD6857686105830F0CF6E99
   476CB07ECAAEA8B5CCC2720479423F8504E783D6712E424C636DAB41203D9EC76F47C4B56F453C42E5626048C24CC585
   F0710514EEF6D4C9644E0721CEAE9F885FBD672742A555095A895C7F0D4E814BEF4D223B13285E95BEDF7357D3545784
   32C1EBB63A6EF1D83E21A08DADA073BF9419C7A3185BB492A13569F262683E7CD86EC66CF671C919789038598EFEC22B
   C8EA1E265A4E0864F9E7253BE32457AC1B186722F3D0FF4AD450D04BA97D5B7DC1AA617DBD25EE8EC912072ABCBF5394
   D08AA276732666D4C349196940BFE869DA909EC03A8E25B23339EE50453CB5F81400B1380CA46AF0FC012CA55F322C1C
   5806E5D76D4CD8308B8FDFE
```
## OpenSSL and RSA Keys
Another way to view the private key in OpenSSL is by using the `rsa` module. While the `asn1parse` module is a general-purpose ASN.1 syntax parser, the `rsa` module recognizes the structure of an RSA key and can correctly display the field names.
```
$ openssl rsa -in private.pem -noout -text
Private-Key: (2048 bit)
modulus:
    00:b2:f5:fd:3f:9f:09:17:11:2c:e4:2f:8b:f8:7e:
    d6:76:e1:52:58:be:44:3f:36:de:af:b0:b6:9b:de:
    24:96:b4:95:ea:ad:1b:01:ca:d8:42:71:b0:14:e9:
    6f:79:38:6c:63:6d:34:85:16:da:74:a6:8a:8c:70:
    fb:a8:82:87:0c:47:b4:21:8d:8f:49:18:6d:df:72:
    72:7b:9d:80:c2:19:11:c3:e3:37:c6:e4:07:ff:b4:
    7c:2f:27:67:b0:d1:64:d8:a1:e9:af:95:f6:48:1b:
    f8:d9:ed:fb:2e:39:04:b2:52:92:68:c4:60:25:6f:
    af:d0:a6:77:d2:98:98:f1:0b:1d:15:12:8a:69:58:
    39:fc:08:ed:d5:84:e8:33:56:15:b1:d1:d7:27:7b:
    e6:5c:53:2d:ca:92:dd:c7:05:03:74:86:8b:11:7e:
    a9:15:49:14:ef:92:92:b8:44:3f:13:69:6e:4f:ad:
    50:de:d6:bd:90:e5:a6:f7:ed:33:be:2e:ce:31:c6:
    dd:7a:42:53:ee:6c:dc:56:78:7d:dd:1d:5c:d7:76:
    61:40:22:db:87:d0:3b:b2:2f:23:28:5b:5a:31:67:
    af:8d:ac:ab:be:a4:00:04:47:13:37:d3:78:1e:8c:
    5c:ca:0e:a5:e2:77:99:b5:10:e4:ef:93:8c:61:ca:
    a6:0d
publicExponent: 65537 (0x10001)
privateExponent:
    00:b2:42:55:00:0a:6a:03:90:18:27:33:35:39:51:
    1e:4f:4c:21:ba:43:cb:b7:2b:f0:a5:10:60:d4:e1:
    71:90:ac:50:a8:71:c5:75:03:98:66:96:d7:cd:fc:
    b8:0d:07:26:ef:e2:d7:6d:ba:55:df:dc:04:25:e0:
    64:cc:75:38:10:03:5c:6a:0f:97:aa:37:ab:39:e7:
    c6:21:5b:c1:e5:95:13:1d:0c:37:82:e5:a1:12:13:
    b5:9f:42:a1:06:7f:8c:f4:3c:53:89:92:d6:be:fd:
    1d:e3:f6:29:3c:e1:8e:cc:11:73:c4:e7:d6:dd:73:
    62:ad:73:23:e7:a2:18:b5:ff:b0:f2:45:eb:79:63:
    27:cc:87:49:3e:dd:13:42:34:ed:5f:3b:14:a4:c4:
    d9:23:74:59:7f:64:a6:d3:cb:2c:10:f0:cd:2d:57:
    e9:9f:58:c8:d2:8f:20:49:d1:43:3c:c4:bd:67:70:
    17:ad:1b:dd:1c:83:cf:b8:fb:7e:8c:8f:dc:f0:b4:
    fb:77:de:7b:82:85:74:9c:ed:fb:fd:68:78:f7:f7:
    93:00:73:f0:f4:2a:dd:cb:a8:38:5d:7e:d0:5c:df:
    ca:a2:a2:ba:75:76:01:72:3a:96:20:1f:ec:cc:2e:
    65:c6:5e:14:f6:5f:1d:34:d6:ec:df:e3:f8:54:01:
    80:01
prime1:
    00:e1:d1:63:89:bf:6e:ff:7a:e4:4f:65:71:06:ed:
    81:c8:1a:48:b5:fb:35:6f:83:dd:4a:22:9e:86:54:
    bd:c0:36:71:6b:bd:9d:46:df:d1:49:81:32:54:50:
    54:95:8a:ca:5c:fd:a7:09:d9:7c:c8:c6:a9:e9:20:
    3d:05:f7:b9:d4:5e:68:5a:19:a5:f5:82:67:fc:b1:
    7f:cf:50:2b:32:cf:ed:b9:4c:ae:a5:8e:e5:f6:3e:
    ba:5f:33:d0:99:46:c8:65:21:32:34:44:10:d3:d6:
    58:74:8b:ca:e2:56:f2:48:96:c2:a9:ad:93:40:d3:
    c8:39:26:52:da:8e:d7:34:6d
prime2:
    00:ca:e1:55:c9:b3:a4:54:6b:5f:c3:cf:4c:c8:0d:
    53:9d:53:1c:40:6b:ac:5e:d8:28:18:e9:77:b4:96:
    f9:f6:14:ce:fb:11:79:e3:bf:bf:ab:22:bc:a7:f8:
    8e:bb:8c:9b:13:27:ae:70:11:32:42:df:f0:86:63:
    70:b6:c7:67:82:db:d5:0d:be:1f:ee:9b:33:16:b9:
    aa:c7:ba:bb:7c:fa:0a:9e:f2:6c:3c:97:6c:f6:2d:
    a8:f4:1e:fe:06:54:58:dc:7c:1c:bc:a7:8f:b1:cb:
    4f:f7:aa:50:d1:16:ce:16:40:95:6a:4e:89:ea:df:
    52:93:fa:a1:3a:23:49:f4:21
exponent1:
    00:bc:3b:93:32:4e:6d:92:ee:78:83:aa:36:66:24:
    f2:8a:bf:46:1e:d3:b0:be:2c:f7:f8:05:15:89:39:
    f8:15:d2:0c:07:58:3e:52:c6:dc:a8:dd:d5:fb:2c:
    1e:e5:ac:94:74:a1:47:6c:d1:6a:cf:dd:b1:e2:4e:
    ea:2f:20:49:39:ba:1c:58:06:8b:2d:34:2f:c4:16:
    9d:48:4d:36:45:1b:c7:b8:2f:30:61:76:d5:3f:c7:
    18:09:a5:a2:5b:32:02:77:32:0d:ac:3d:94:9d:50:
    4d:d9:90:71:64:ec:3e:f7:bd:1b:b4:de:a8:21:60:
    a7:c4:e3:aa:2a:de:e8:8a:9d
exponent2:
    29:15:e9:21:a7:d7:a7:a0:f7:0b:d8:77:5c:2c:16:
    ba:cd:91:f3:19:db:16:79:ff:e4:cb:a3:0a:57:68:
    d7:84:ef:45:b9:0c:4e:2b:0e:cd:c1:83:23:21:1b:
    06:b0:3a:d7:6e:39:cd:48:2e:3d:8c:cc:50:ea:e2:
    70:a1:81:3c:e6:f8:06:88:72:3f:07:ff:18:a3:11:
    0a:d1:ae:16:69:2c:ad:73:ba:a7:aa:a2:ce:58:00:
    d7:2f:4f:92:48:92:96:54:2c:1d:a8:71:59:38:2b:
    41:a4:a4:29:33:cd:18:84:8b:bd:b3:9a:0a:8e:9f:
    52:88:77:0e:27:07:5b:01
coefficient:
    3a:b4:e3:b8:41:ab:23:45:15:bf:0a:8d:2e:40:fb:
    6e:95:38:97:02:d8:34:47:4e:9a:d8:49:12:4d:c6:
    c1:d3:42:73:8d:4e:75:10:26:5d:f6:b7:44:eb:aa:
    4a:88:a7:99:53:46:be:ef:04:7d:b0:24:ce:8b:2a:
    4e:39:23:b0:56:63:89:94:8a:b0:bb:b0:31:87:97:
    70:da:14:f4:41:8a:eb:75:ae:98:34:91:22:a2:d9:
    53:51:17:b0:5b:ef:93:8a:12:11:a3:be:6e:88:29:
    57:bc:2a:5f:1d:e5:ca:50:c2:6f:42:ee:0a:38:3a:
    2a:2b:63:40:d5:2e:1a:36
```
Fields here are the same as in the ASN.1 structure, but in this representation, the specific values of the key components are presented more clearly. Compare these two representations and ensure that the field values match.

Interesting fact: try to explore the historical reasons for choosing 65537 as the commonly accepted public exponent (see the `publicExponent` section).
