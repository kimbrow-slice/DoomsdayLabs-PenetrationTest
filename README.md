# Doomsday Labs Penetration Testing Report

This repository contains a penetration testing report and supporting documents for BSidesKC Pentration Testing Training 2024 "Doomsday Labs".

## Overview

- **Objective**: Identify and exploit network vulnerabilities.
- **Methodology**: Combination of OSINT, SMB Relay attacks, and Credential Dumping.

## Repository Structure

- `/docs` - Comprehensive reports and findings.
- `/tools` - Scripts and tools used during the tests.
- `/data` - Additional data (ensure there is no sensitive information).

## Tools Used

- Nmap
- Responder
- Hashcat

See individual folders in `/tools` for specific configurations and usage.

## Report

For a full report, see [the detailed report here](docs/report.md).

## Findings

Detailed findings are documented separately in the `/docs/findings` directory.

- [SMB Relay Attacks](docs/findings/smb_relay.md)
- [Credential Dumping](docs/findings/credential_dumping.md)
- [Active Directory Exploitation](docs/findings/active_directory.md)
