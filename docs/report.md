# Comprehensive Penetration Testing Report for Doomsday Labs

## Executive Summary
This document summarizes the penetration test conducted against Doomsday Labs' network spanning the IP range 10.100.1.2 to 10.100.1.7. The objective was to identify and exploit vulnerabilities within the network to improve security measures.

## Test Environment
- **Scope**: IPs 10.100.1.2 to 10.100.1.7
- **Tools Used**: Nmap, Crackmapexec, Responder, Hashcat, Impacket, Bloodhound, Neo4j

## Key Exploits and Findings

### SMB Relay and Hash Capture
- **Method**: Employed Crackmapexec for SMB Relay attacks to capture NTLM hashes.
- **Outcome**: Credentials obtained were leveraged to infiltrate network systems, indicating significant security weaknesses.

### Credential Dumping
- **Technique**: Utilized Hashcat to decrypt captured NTLM hashes, gaining plaintext passwords.
- **Impact**: The extracted credentials facilitated unauthorized access, highlighting susceptibility to credential stuffing attacks.

### Active Directory Exploitation
- **Tools**: Bloodhound and Neo4j were used to analyze trust relationships and AD configurations.
- **Results**: Successfully escalated privileges to Domain Admin, exposing severe vulnerabilities.

### Bloodhound Analysis
- **Objective**: Map out Active Directory trust relationships and identify potential privilege escalation paths.
- **Process**:
    - Used Bloodhound with the Neo4j database to visualize AD environments and pinpoint misconfigurations.
    - Specific commands included:
        ```bash
        bloodhound-python -c All -d domain.com -u username -p password -gc path_to_neo4j
        ```
    - This allowed us to identify high-risk targets within the network and plan our attack vectors more effectively.

## Methodology
The approach ranged from initial reconnaissance using passive data gathering to active exploitation of identified vulnerabilities, using sophisticated tools and techniques.

## Detailed Findings and Recommendations
- **SMB Relay Attack**: 
  - *Recommendation*: Implement SMB signing across the board to mitigate such vulnerabilities.
- **Credential Security**:
  - *Recommendation*: Revise password policies and perform routine security audits.
- **Active Directory and Bloodhound Insights**:
  - *Recommendation*: Regularly update AD configurations, use Bloodhound to continuously monitor for potential security gaps, and address findings promptly.

## Conclusion
Immediate and focused actions are recommended to rectify the identified security issues, significantly enhancing the network's defenses.

## Appendix: Command Logs and Outputs
- **SMB Relay Setup**: `crackmapexec smb 10.100.1.2-7 --gen-relay-list targets.txt`
- **Hash Cracking**: `hashcat -m 5600 example.hashes /usr/share/wordlists/rockyou.txt --force`
- **Bloodhound Scanning**: 
    ```bash
    sudo bloodhound-python -c All -d domain.com -u username -p password --zip output.zip
    neo4j console
    ```
