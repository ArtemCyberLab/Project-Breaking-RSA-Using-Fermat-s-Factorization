Breaking RSA Using Fermat's Factorization Method
Project Overview
This project demonstrates a practical attack on poorly implemented RSA keys using Fermat's factorization method. The scenario involves a vulnerable server generating RSA keys where primes p and q are chosen too close together, making factorization feasible and compromising the private key.

Objective
The goal was to:

Recover the private RSA key from the public key

Use it to gain SSH access

Retrieve the system flag

This showcases a real-world cryptanalysis attack, highlighting why secure RSA key generation is critical.

Methodology
1. Reconnaissance
Port scanning with nmap revealed:

nmap -p- -T4 10.10.124.176
Open ports: 22/tcp (SSH), 80/tcp (HTTP)

Directory enumeration with gobuster:

gobuster dir -u http://10.10.124.176 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
Discovered /development directory containing id_rsa.pub

2. Public Key Retrieval
Downloaded the public key:

wget http://10.10.124.176/development/id_rsa.pub
3. Fermat's Factorization Implementation
Created Python script (break_rsa.py) to:

Factorize the modulus n using Fermat's method

Calculate private exponent d

Reconstruct and save the private key

Key components:

python
def factorize(n):
    a = isqrt(n)
    while True:
        b_sq = a*a - n
        b = isqrt(b_sq)
        if b*b == b_sq:
            return a+b, a-b
        a += 1
4. Private Key Recovery
Executed the script:

python3 break_rsa.py
Successfully obtained:

Prime factors p and q

Private exponent d

PEM-formatted private key (id_rsa)

5. SSH Access
Secured the key and connected:

chmod 400 id_rsa
ssh root@10.10.124.176 -i id_rsa
6. Flag Retrieval
bash
cat flag
Flag: breakingRSAissuperfun20220809134031

Security Implications
This attack demonstrates:

Critical vulnerability when RSA primes are too close together

How mathematical weaknesses can lead to complete system compromise

The importance of proper key generation practices
