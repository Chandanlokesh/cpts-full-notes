
---


#### **What is a Zone Transfer?**

- A **DNS zone transfer** is a process of copying all DNS records within a domain (zone) from one name server to another.
    
- It ensures consistency and redundancy across DNS servers.
    
- If misconfigured, unauthorized parties can request a zone transfer and obtain the entire zone file, revealing sensitive DNS information.
![Diagram showing data transfer between secondary and primary servers. Includes steps: XML Request, XML Record, loop for retries, XML Report, and AOK (Acknowledgment).](https://mermaid.ink/svg/pako:eNqNkc9qwzAMxl9F-JSx7gV8KISWXcY2aHYYwxdjK39obGWKvBFK333ukg5aGNQnW9b3Q_q-g3LkUWk14mfC6HDb2YZtMBHyGdFR9JanCvkL-WG9vh-4C38FDeX74w52J-0oUHxQRHhjG8ca-W5mXAgy4YqpoXotM8EReygqsSxANZRJWuJOpoXSEw0gC3ku3QTfvlQLfBZh9DeOdbELbCgMPQr-58u1LZsnKEq3j_Tdo28wYJS8iVqpgBxs57PjhxPLKGnzr1E6XzNxb5SJx9xnk1A1Rae0cMKVYkpNq3Rt-zG_0uCtnLM6t6DvhPh5zvM31uMPG8qm-A)

#### **Zone Transfer Process (AXFR)**

1. **Zone Transfer Request (AXFR)**:
    
    - Initiated by a secondary DNS server requesting a copy of the zone from the primary DNS server.
        
    - The request uses the AXFR (Full Zone Transfer) type.
        
2. **SOA Record Transfer**:
    
    - The primary server sends the Start of Authority (SOA) record to the secondary server, which includes vital information (e.g., serial number).
        
3. **DNS Records Transmission**:
    
    - The primary server sends the complete list of DNS records (A, MX, CNAME, NS, etc.) to the secondary server.
        
4. **Zone Transfer Complete**:
    
    - After transmitting all records, the primary server indicates the transfer is complete.
        
5. **Acknowledgement (ACK)**:
    
    - The secondary server sends an acknowledgment, confirming receipt of the zone data.
        

#### **The Zone Transfer Vulnerability**

- **Misconfigured DNS Servers**:
    
    - Historically, many DNS servers allowed any client to request a zone transfer.
        
    - Malicious actors could exploit this to gather a comprehensive list of subdomains, IP addresses, and other sensitive DNS data.
        
- **Information Exposed**:
    
    - **Subdomains**: Unlisted subdomains not visible on the main website (e.g., development, staging environments).
        
    - **IP Addresses**: Associated IPs for potential targets.
        
    - **Name Servers**: Details about the authoritative name servers, potentially revealing hosting providers and misconfigurations.
        

#### **Remediation**

- **Modern DNS Servers**:
    
    - Most now restrict zone transfers to trusted secondary servers.
        
    - Administrators should ensure zone transfers are not publicly accessible.
        
- **Common Mitigations**:
    
    - Restrict access to trusted secondary servers only.
        
    - Regularly audit DNS configurations for misconfigurations.
        

#### **Exploiting Zone Transfers**

- **Dig Command**:
    
    - You can use the `dig` command to test for zone transfer vulnerabilities:
        
    
    ```bash
    dig axfr @<dns-server> <domain>
    ```
    
    - Example:
        
    
    ```bash
    dig axfr @nsztm1.digi.ninja zonetransfer.me
    ```
    
    - If misconfigured, the DNS server will respond with the full zone record, including all subdomains and associated data.
        

#### **Example Output from a Zone Transfer**

```bash
zonetransfer.me.  7200 IN SOA nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.  300 IN HINFO "Casio fx-700G" "Windows XP"
zonetransfer.me.  7200 IN MX 0 ASPMX.L.GOOGLE.COM.
zonetransfer.me.  7200 IN A 5.196.105.14
zonetransfer.me.  7200 IN NS nsztm1.digi.ninja.
```

- **Misconfigured Server**:
    
    - When misconfigured, the full zone records will be exposed, which can include subdomains and IP addresses.
        

#### **Best Practices**

- **Zone Transfer Restrictions**: Limit access to zone transfers only to trusted servers to prevent unauthorized access.
    
- **Regular Audits**: DNS servers should be regularly audited to ensure proper security measures are in place, such as restricting AXFR requests to specific servers.
    

---

