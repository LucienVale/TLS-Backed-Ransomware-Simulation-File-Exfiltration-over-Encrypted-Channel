# TLS-Backed-Ransomware-Simulation-File-Exfiltration-over-Encrypted-Channel
Summary

This project simulates a ransomware scenario involving the encryption of .txt files and secure transmission to a TLS-encrypted Command & Control (C2) server. The exercise was a hybrid implementation combining:

•	Chapter 5: Ransomware and Encryption
 
•	Chapter 6: TLS and Network Cryptography

from Ethical Hacking: A Hands-On Introduction to Breaking In by Daniel G. Graham.

My addition included chaining the encryption step with TLS-based exfiltration, emulating how an attacker could evade detection while extracting files securely.

⸻

##Objectives
•	Encrypt specific .txt files in a given directory.

•	Send encrypted payloads over a secure TLS connection.

•	Simulate a C2 server that receives and stores exfiltrated files.

•	Combine cryptography, ransomware logic, and TLS socket programming.

•	Explore TCP socket communication and simulate attacker methodology.

⸻

Tools & Technologies

•	Python 3

•	Kali Linux

•	cryptography Python package (pip install cryptography)

•	TLS via ssl and socket

•	Manual TLS certificate and key generation using openssl

⸻

System Flow Overview
	
 1.	RansomwareSim.py

•	Scans a user-defined directory for .txt files.

•	Encrypts each file using AES in CTR mode.

•	Sends the encrypted .enc file to a TLS server.

<img width="766" alt="Screenshot 2025-05-17 at 12 13 32 AM" src="https://github.com/user-attachments/assets/8be2c776-12c6-462d-b8a8-2c90b13f184c" />

2.	tls_c2_server.py

•	Listens on port 4443 over TLS.

•	Receives incoming encrypted files.

•	Writes them to ~/Desktop/received_files/ for 
review. 

<img width="736" alt="Screenshot 2025-05-17 at 12 14 01 AM" src="https://github.com/user-attachments/assets/15f86a3a-4a88-4232-a252-8819d6fcb5d9" />


⸻

Encryption Logic
	
  •	AES-CTR symmetric encryption.

•    Hardcoded key + random IV per file.

•	Encrypted file format: IV + ciphertext. 

cipher = Cipher(algorithms.AES(KEY), modes.CTR(IV))encryptor = cipher.encryptor()encrypted_data = encryptor.update(data) + encryptor.finalize() 

TLS Socket Layer

•	Used ssl.create_default_context() with CERT_NONE (for simulation).

•	The client sends:

1.	Filename as raw string.
	
 2.	Encrypted contents of file.

•	The server reconstructs and stores the file.

## Screenshots

Below are screenshots of the ransomware script in action, showing successful encryption and exfiltration to the TLS C2 server.

RansomwareSim.py:

<img width="995" alt="Screenshot 2025-05-17 at 12 11 30 AM" src="https://github.com/user-attachments/assets/561b2a7c-6abc-4e7b-848e-7a15b8d9a559" />

tls_c2_server.py:

<img width="988" alt="Screenshot 2025-05-17 at 12 11 46 AM" src="https://github.com/user-attachments/assets/166e2e02-7429-4b17-811b-22fcb29f19a4" />

Received:

<img width="611" alt="Screenshot 2025-05-17 at 12 12 09 AM" src="https://github.com/user-attachments/assets/b613f388-2930-4c25-816c-79e8296cb4d5" />



Example Run

On the server:

python3 tls_c2_server.py

Output:

[+] TLS C2 server listening on port 4443 ...

[+] Receiving file: testfolder.txt.enc

[+] File saved to /home/kali/Desktop/received_files/testfolder.txt.enc

On the ransomware client:

python3 RansomwareSim.py

Output:

[!] Found: /home/kali/Desktop/testfolder/testfolder.txt

[+] Encrypted_data /home/kali/Desktop/testfolder/testfolder.txt

[+] Sent /home/kali/Desktop/testfolder/testfolder.txt.enc to c2 server

[!] Scanning directory: /home/kali/Desktop/testfolder

Result

•	.txt files in the victim directory were encrypted and renamed with a .enc suffix.

•	The encrypted files were successfully transferred over TLS to the C2 server.

•	Server received and saved the payloads.

•	TLS ensured encrypted traffic, simulating how real ransomware might operate.

## Future Enhancements

•    Implement RSA to encrypt the AES key before transmission, simulating hybrid encryption techniques used in real-world ransomware.

•    Introduce Diffie-Hellman key exchange to dynamically negotiate session keys between client and server.

•     Add metadata obfuscation to make the encrypted files less suspicious.

•     Expand the search pattern to include additional file types (e.g., `.docx`, `.pdf`) and subdirectory recursion.

•     Include a decryption routine with private key handling for safe recovery in controlled environments.
 
