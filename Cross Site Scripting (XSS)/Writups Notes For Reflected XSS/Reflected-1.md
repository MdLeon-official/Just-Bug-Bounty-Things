### 1. [Reflected XSS on Microsoft subdomains](https://infosecwriteups.com/reflected-xss-on-microsoft-com-subdomains-4bdfc2c716df)

##### **What Happended**
Reflected XSS in Microsoft's .NET subdomains caused by improper HTML encoding of "cookieless" session identifiers. 
The researcher found it by scanning microsoft.com subdomains and testing .NET-specific ~/ URL resolution patterns.

##### **Tools Used**
- **[Assetfinder](https://github.com/tomnomnom/assetfinder)** - Subdomain enumeration
- **[Httprobe](https://github.com/tomnomnom/httprobe)** - Check live hosts  
- **[Cookieless](https://github.com/RealLinkers/cookieless)** - Custom tool for testing .NET cookieless sessions
  
##### **Payload Used**
*https://gallery.technet.microsoft.com/(A(%22onerror='alert%601%60'testabcd))/*

- The Researcher's Method: Recon: assetfinder microsoft.com | httprobe → Find live subdomains -> Filter: cookieless tool to detect .NET cookieless patterns -> Test: Try breaking out of the session ID context with XSS payloads -> Verify: Found 4 vulnerable subdomains

##### **How to Prevent**
1. HTML encode ALL user-controlled data in URLs
2. Implement Content Security Policy (CSP)
3. Avoid cookieless sessions when possible
4. Treat URL parameters as untrusted input
