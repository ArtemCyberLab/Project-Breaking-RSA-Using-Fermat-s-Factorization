Project Summary
I successfully cracked a weak RSA key because the server used two prime numbers that were too close together. This allowed me to quickly factorize the key and gain access to the system.

How It Works (In Simple Terms)
Found the public key
Through scanning, I discovered the id_rsa.pub file on the server.

Factorized the key
Used Fermat's method which works efficiently when primes p and q are close to each other.

Calculated the private key
Knowing p and q, I was able to reconstruct the full private key.

Gained server access
Used the obtained key to SSH into the system.

Step-by-Step Process
Discovered open ports:

nmap -p- 10.10.124.176
Found web server (port 80) and SSH (port 22)

Retrieved the public key:

bash
wget http://10.10.124.176/development/id_rsa.pub
Ran the cracking program:

python
# Core part of the code:
a = int(sqrt(n)) + 1
while True:
    b = sqrt(a*a - n)
    if b.is_integer():
        p = a + b
        q = a - b
        break
    a += 1
Obtained and saved the private key to id_rsa

Connected to the server:

ssh root@10.10.124.176 -i id_rsa
Found the flag in the flag file

Why It Worked
The server used two prime numbers that were too close together. For example:

p = 100003

q = 100019

Fermat's method quickly finds such numbers, breaking RSA security.

Protection Recommendations
Always generate keys using reliable programs

Use primes that differ significantly

Verify that |p-q| is sufficiently large

Obtained flag:
breakingRSAissuperfun20220809134031
