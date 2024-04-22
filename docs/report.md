# Penetration Testing Report

## Summary
This document outlines the penetration testing activities for Doomsday Labs' network within the IP range 10.100.1.2 to 10.100.1.7 aimed at identifying and mitigating vulnerabilities.

## Test Environment
- **Scope**: IPs 10.100.1.2 to 10.100.1.7
- **Tools Used**: Nmap, Responder, Hashcat, Impacket
- **Credentials Used**: Initial credentials found via OSINT - info@doomsdaylabs.net:WhenTomorrowisNot@nOption2024

## Key Findings & Exploits

### SMB Relay and Credential Harvesting
- **Hash Captured**: `filemaker::DOOMSDAYLABS:322e0b51ca281c55:F1F9E4FF7872D084AC0E2CB11DE7D2C0`
- **Finding**: Successfully performed an SMB Relay attack against 10.100.1.3, which led to the capture of NTLMv2-SSP credentials.
- **Impact**: Unauthorized access was gained to network shares, which could lead to data exfiltration.

### Credential Dumping
- **Technique Used**: NetNTLM hash `filemaker::DOOMSDAYLABS:322e0b51ca281c55:F1F9E4FF7872D084AC0E2CB11DE7D2C0` was cracked using Hashcat to reveal the plaintext password.
- **Impact**: Further unauthorized access was obtained, allowing for lateral movement within the network.

### Active Directory Exploitation
- **Finding**: Leveraged cracked credentials to exploit poor Active Directory configurations and obtained administrative privileges.
- **Impact**: Full administrative control over the network was achieved.

## Methodology
The test followed a structured approach from initial reconnaissance using OSINT to exploitation of identified vulnerabilities.

## Findings and Recommendations
- **SMB Relay Attack**: 
  - *Recommendation*: Enable SMB signing and disable SMBv1.
- **Credential Security**: 
  - *Recommendation*: Improve password policies and conduct regular audits.
- **Active Directory Configuration**:
  - *Recommendation*: Regularly update and patch AD environments and conduct security reviews.

## Conclusion
Immediate action is recommended to address the vulnerabilities identified during this test to enhance the security posture of Doomsday Labs.

## Appendix: Tools and Commands
- **Nmap**: `nmap -sT -oN -sV -sC 10.100.1.2-10.100.1.7`
- **Responder**: `responder -I eth0 -w -rf`
- **Hashcat**: `hashcat -m 5600 hashfile.txt -r rules.txt -o cracked.out`
