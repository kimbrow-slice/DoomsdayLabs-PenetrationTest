# Tools Documentation

This directory contains detailed descriptions and usage instructions for all tools used during the penetration testing process.

## Nmap

Nmap (Network Mapper) is a free and open-source network scanner used to discover hosts and services on a computer network by sending packets and analyzing the responses.

### Usage

```
nmap -sT -oN -sV -sC [target IP]
```

## Responder

Responder is a package that contains LLMNR, NBT-NS and MDNS posioner. We utilized this tool, in aid of a LNK file being dropped, to capture NTLMv2 hashes from a local administrator which was later decrypted. 

### Usage

```
responder -I eth0 -w -rf
```

## Hashcat 

Hashcat is a free open-source software that is considered the worlds fastest password recovery tool. This is often used to crack encrypted passwords or hashes to gain priviledge escalation. 

### Usage

```
hashcat -m 5600 hashfile.txt /usr/share/wordlists/rockyou.txt -r rules.txt -o hashfile.out
```
