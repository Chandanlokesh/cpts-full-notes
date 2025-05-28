
## Introduction

Subdomain Brute-Force Enumeration is an active method of discovering subdomains by systematically testing potential subdomain names against a target domain. This approach helps uncover hidden or less-known subdomains that may contain sensitive information or vulnerable services.

## Key Steps in Subdomain Bruteforcing

### 1. Wordlist Selection

- **General-Purpose Lists:** Contain common subdomain names (e.g., dev, staging, blog).
    
- **Targeted Lists:** Focus on industry-specific or technology-specific subdomains.
    
- **Custom Lists:** Manually created based on known patterns or keywords.
    

### 2. Iteration and Querying

- A script or tool appends each word in the wordlist to the main domain (e.g., test.example.com).
    

### 3. DNS Lookup

- For each potential subdomain, a DNS query (A or AAAA record) is performed.
    

### 4. Filtering and Validation

- Successfully resolved subdomains are listed as valid.
    
- Further validation can include accessing subdomains to ensure they are active.
    

## Tools for Subdomain Bruteforcing

|Tool|Description|
|---|---|
|**dnsenum**|Comprehensive DNS enumeration with brute-force support.|
|**fierce**|User-friendly with recursive search and wildcard detection.|
|**dnsrecon**|Versatile with customizable output formats.|
|**amass**|Integrates multiple sources for extensive subdomain discovery.|
|**assetfinder**|Lightweight tool for quick subdomain discovery.|
|**puredns**|Powerful, flexible, and efficient DNS brute-forcing.|

## Using dnsenum for Subdomain Bruteforcing

- **dnsenum** is a popular tool for DNS reconnaissance and subdomain brute-forcing.
    
- It can automate various tasks, including:
    
    - DNS record retrieval (A, NS, MX, TXT)
        
    - Zone transfer attempts
        
    - Subdomain brute-forcing
        

### Example Command

```bash
$ dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -r
```

### Explanation

- **--enum:** Activates all enumeration features.
    
- **-f:** Specifies the wordlist for brute-forcing.
    
- **-r:** Enables recursive subdomain brute-forcing.
    

### Sample Output

```
Brute forcing with /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt:
www.inlanefreight.com. 134.209.24.248
support.inlanefreight.com. 134.209.24.248
```

## Best Practices

- Use targeted or custom wordlists for efficient discovery.
    
- Always verify discovered subdomains.
    
- Avoid excessive brute-forcing that may trigger security mechanisms.