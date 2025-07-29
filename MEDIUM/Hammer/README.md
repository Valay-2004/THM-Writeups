# TryHackMe Hammer Writeup


## Reconnaissance

The first step was scanning the target machine to identify open services.

I ran **RustScan** and **Nmap** as follows:
```
rustscan -a <target-ip> --ulimit 5000
nmap -sC -sV -p- <target-ip>
``` 
<sub> Rustscan is FASTER than NMAP 🫡


**Results:**

- **22/tcp**: Open SSH service
- **1337/tcp**: HTTP web server hosting a web application

The main attack surface was the web service on port **1337**.

![RustScan and Nmap Scan Results](./images/rustscan_nmap.png)
<!-- Insert terminal screenshot of RustScan and Nmap output -->

---

## Initial Web Analysis

Visiting `http://<target-ip>:1337` revealed a login page but no known credentials.

Inspecting the page source uncovered a consistent pattern in directory names: all were prefixed with `hmr_` (e.g., `hmr_css`).

This observation suggested that hidden directories might also share this prefix.

---

## Directory Enumeration

To enumerate these directories, I:

1. Copied a large `/usr/share/wordlists/dirb/big.txt` wordlist, prepending `hmr_` to each line:

