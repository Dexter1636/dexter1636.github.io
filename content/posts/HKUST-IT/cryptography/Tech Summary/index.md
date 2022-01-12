+++
title = "Tech Summary"
description = ""
math = true
type = ["posts","post"]
tags = [
    "cryptography",
    "web security",
    "computer science",
]
date = "2021-12-27"
categories = [
    "Computer Science",
    "HKUST-IT",
    "cryptography",
]
series = ["CSIT5710"]
[ author ]
  name = "Dexter"

+++

### Confidentiality Service

$$
Alice \rightarrow E_k(m) \rightarrow Bob
$$



### Authentication and Data Integrity

In PGP and S/MIME,
$$
Alice \rightarrow m||Alice's\ digital\ signiture\ on\ m \rightarrow Bob
$$
In most real-world security systems,
$$
Alice \rightarrow m||h_k(m) \rightarrow Bob
$$


### Providing Mutual Authentication

Type-1: Kerberos-like protocol,
$$
Alice \rightarrow E_k(ID_A||ID_B||timestamp) \rightarrow Bob \\
Alice \leftarrow  E_k(ID_B||ID_A||timestamp) \leftarrow  Bob
$$
where $k$ is a pre-shared secret key.

Type-2: challenge-response protocol,
$$
Alice \rightarrow E_{K_e^B}(N_1) \rightarrow Bob \\
Alice \leftarrow  N_1 \leftarrow Bob
$$
This is to allow Alice to authenticate Bob. Authentication in the other direction is similar.



### Establishing a Common Secret Key

Type-1: digital-envelop method,
$$
Alice \rightarrow E_{k_e^B}(k) \rightarrow Bob
$$
Type-2: Diffie-Hellman protocol,

<img src="assets/95E11BDC2AFBE38F3DBAE243B46C046B.png" alt="img" style="zoom:30%;" />

where $p$ and $\alpha$ are publicly known.

### Reference

Lecture slides of HKUST CSIT5710, by Prof. Cunsheng DING

