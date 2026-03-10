---
name: shodan-recon-scanner
description: Shodan-powered reconnaissance and vulnerability scanner. Search for exposed services, CVE vulnerabilities, and perform security recon on IPs/domains. Triggers: shodan, recon, cve-scan, vuln-scan, exposed-services, device-search, ip-recon.
---

# Shodan Skill

Search engine for Internet-connected devices. Find exposed services, vulnerabilities, and perform security reconnaissance.

## CLI Commands

### Setup & Info
```bash
shodan init <api_key>        # Initialize with API key
shodan info                  # Show account info and credits
shodan myip                  # Show your external IP
```

### Search & Discovery
```bash
shodan search <query>        # Search Shodan database
shodan host <ip>             # Get detailed host information
shodan domain <domain>       # Get domain information
shodan count <query>         # Count search results
shodan stats <query>         # Get query statistics
shodan trends <query>        # Historical trend data
```

### Scanning & Analysis
```bash
shodan scan <target>         # Scan IP/netblock (uses credits)
shodan honeyscore <ip>       # Check if IP is a honeypot (0.0-1.0)
shodan download <file> <q>   # Download results to file
shodan parse <file>          # Parse downloaded results
```

## Search Filters

Combine filters with spaces for precise queries:

```bash
port:21                      # Filter by port
country:US                   # Filter by country code
city:"Los Angeles"           # Filter by city
os:Windows                   # Filter by operating system
product:Apache               # Filter by product/service
version:2.4                  # Filter by version
vuln:CVE-2021-44228         # Filter by CVE
has_vuln:true               # Has any vulnerability
ssl:True                     # SSL/TLS enabled
org:"Amazon"                 # Filter by organization
asn:AS15169                  # Filter by ASN
net:192.168.0.0/16          # Filter by network range
hostname:example.com         # Filter by hostname
```

## CVE Vulnerability Scanning

### Quick CVE Scans
```bash
# Find Log4Shell vulnerable servers
shodan search 'vuln:CVE-2021-44228'

# Find BlueKeep vulnerable Windows machines
shodan search 'vuln:CVE-2019-0708 port:3389'

# Find EternalBlue vulnerable SMB
shodan search 'vuln:CVE-2017-0144 port:445'

# Find Spring4Shell
shodan search 'vuln:CVE-2022-22965'

# Find Heartbleed OpenSSL
shodan search 'vuln:CVE-2014-0160 ssl:true'

# Find Shellshock Bash
shodan search 'vuln:CVE-2014-6271'

# Any device with vulnerabilities
shodan search 'has_vuln:true'
```

### CVE by Product
```bash
# Apache vulnerabilities
shodan search 'product:Apache has_vuln:true'

# Nginx vulnerabilities
shodan search 'product:nginx has_vuln:true'

# WordPress vulnerabilities
shodan search 'product:WordPress has_vuln:true'

# Cisco vulnerabilities
shodan search 'org:Cisco has_vuln:true'
```

### CVE Scan Workflow
```bash
# 1. Count vulnerable devices first (free)
shodan count 'vuln:CVE-2021-44228 country:US'

# 2. Get statistics breakdown
shodan stats --facets org,product 'vuln:CVE-2021-44228'

# 3. Download results for analysis
shodan download log4shell.json.gz 'vuln:CVE-2021-44228'

# 4. Parse and extract IPs
shodan parse --fields ip_str,port,org log4shell.json.gz
```

## Common CVE Quick Reference

| CVE | Name | Search Query |
|-----|------|--------------|
| CVE-2021-44228 | Log4Shell | `vuln:CVE-2021-44228` |
| CVE-2019-0708 | BlueKeep | `vuln:CVE-2019-0708 port:3389` |
| CVE-2017-0144 | EternalBlue | `vuln:CVE-2017-0144 port:445` |
| CVE-2022-22965 | Spring4Shell | `vuln:CVE-2022-22965` |
| CVE-2014-0160 | Heartbleed | `vuln:CVE-2014-0160 ssl:true` |
| CVE-2014-6271 | Shellshock | `vuln:CVE-2014-6271` |
| CVE-2017-5638 | Struts2 | `vuln:CVE-2017-5638` |
| CVE-2020-0796 | SMBleed | `vuln:CVE-2020-0796 port:445` |

## Example Usage

### Basic Reconnaissance
```bash
# Find Apache servers in US
shodan search "apache country:US"

# Find exposed MongoDB databases
shodan search "product:MongoDB port:27017"

# Find devices with specific vulnerability
shodan search "vuln:CVE-2021-44228"

# Find webcams
shodan search "webcam has_screenshot:true"
```

### Host Analysis
```bash
# Get detailed info about an IP
shodan host 8.8.8.8

# Check if IP is a honeypot
shodan honeyscore 192.168.1.1

# Get domain info
shodan domain example.com
```

### Scanning
```bash
# Scan a single IP
shodan scan 192.168.1.1

# Scan a network range
shodan scan 192.168.1.0/24

# Check scan status
shodan scan list
```

### Data Export
```bash
# Download search results
shodan download results.json.gz "apache country:US"

# Parse downloaded data
shodan parse --fields ip_str,port,org results.json.gz
```

### Statistics
```bash
# Count results without using credits
shodan count "nginx"

# Get statistics breakdown
shodan stats --facets country,org "nginx"

# View trends over time
shodan trends "apache"
```

## Tips

- **Combine filters** for precise results: `product:nginx country:US has_vuln:true`
- **Use quotes** for multi-word values: `city:"New York"`
- **Check credits** before large scans: `shodan info`
- **Download first** for large datasets, then parse offline
- **Honeyscore** helps avoid honeypots (>0.5 = likely honeypot)
- **Count before search** to estimate result size
- **Use facets** with stats to understand data distribution

## API Key

Requires API key from https://account.shodan.io

Initialize once:
```bash
shodan init YOUR_API_KEY_HERE
```

Key is stored in `~/.shodan/api_key` and persists across sessions.

## Common Use Cases

- **Vulnerability assessment**: Find devices with specific CVEs
- **Attack surface mapping**: Discover exposed services in your org
- **Threat intelligence**: Track malicious infrastructure
- **IoT discovery**: Find connected devices by product/vendor
- **SSL/TLS analysis**: Find weak crypto implementations
- **Honeypot detection**: Verify targets before engagement
