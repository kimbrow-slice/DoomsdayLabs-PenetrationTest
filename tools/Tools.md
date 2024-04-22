# Tools Documentation

This directory contains detailed descriptions and usage instructions for all tools used during the penetration testing process.

## Nmap

Nmap (Network Mapper) is a free and open-source network scanner used to discover hosts and services on a computer network by sending packets and analyzing the responses.

### Usage

```bash
nmap -sT -oN -sV -sC [target IP]

responder -I eth0 -w -rf

hashcat -m 5600 hashfile.txt /usr/share/wordlists/rockyou.txt -r rules.txt -o hashfile.out

