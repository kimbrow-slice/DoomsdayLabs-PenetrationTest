# Comprehensive Penetration Testing Report

## Executive Summary

This document details the penetration testing activities for Doomsday Labs' network, scoped within IP range 10.100.1.2 to 10.100.1.7. The purpose was to uncover vulnerabilities and evaluate the network's security risks.

## Test Environment

- **Scope**: IPs 10.100.1.2 to 10.100.1.7
- **Tools Used**: Nmap, Responder, Hashcat, Impacket

## Key Findings & Exploits

### SMB Relay and Credential Harvesting

- **Finding**: Successfully executed SMB Relay attack against 10.100.1.3 to capture NTLMv2-SSP credentials.
- **Impact**: Gained unauthorized access to network shares, leading to potential data breaches.

### Credential Dumping

- **Technique**: Used Responder and hashcat for capturing and cracking NetNTLM hashes.
- **Impact**: Unauthorized access escalation and potential for widespread network compromise.

### Active Directory Exploitation

- **Finding**: Exploited poor configurations and weak policies in Active Directory.
- **Impact**: Gained administrative access, compromising the entire network.

## Methodology

Outlined the approach from reconnaissance to exploitation, detailing each step and tool used.

## Detailed Findings and Recommendations

Each vulnerability is discussed with potential impacts and specific recommendations for mitigation.

## Conclusion

Recommendations for improving network security by addressing the identified vulnerabilities.

## Appendix: Tools and Commands

Descriptions and usage examples for all tools used during the testing.
