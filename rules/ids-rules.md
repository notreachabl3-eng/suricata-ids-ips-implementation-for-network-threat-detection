# Suricata custome rule file

### ICMP Ping detection
alert icmp any any -> $HOME_NET any (msg:"Alert: ICMP flood detected"; itype:8; classtype:icmp-event; threshold:type both, track by_dst, count 3, seconds 3; sid:1000001; rev:1;)

---

### Nmap Scan detection
alert tcp any any -> $HOME_NET any (msg:"Alert: Nmap Scan Detected"; flags:S; window:1024; flow:to_server; classtype:network-scan; threshold:type both, track by_src, count 10, seconds 10; sid:1000002; rev:2;)

---
  
### Web enumeration detection
alert http any any -> $HOME_NET any (msg:"Alert: Possible Web Enumeration"; flow:to_server,established; http.user_agent; pcre:"/(gobuster|Mozilla/4.0|Gecko|Fuzz)/V"; classtype:web-application-attack; threshold:type both, track by_src, count 20, seconds 10; sid:1000003; rev:1;)

---
  
### HTTP Brute Force detection
alert http any any -> $HOME_NET any (msg:"Alert: Possible HTTP Brute Force"; http.method; content:"POST"; http.uri; content:"login"; classtype:suspicious-login; threshold:type threshold, track by_src, count 10, seconds 10; sid:1000004; rev:1;)

---
  
### FTP Bruteforce detection
alert ftp any any -> $HOME_NET 21 (msg:"Alert: Possible FTP Bruteforce"; flow:to_server,established; classtype:suspicious-login; threshold:type both, track by_src, count 10, seconds 10; sid:1000005; rev:2;)

---
  
### SSH Bruteforce detection
alert ssh any any -> $HOME_NET 22 (msg:"Alert: Possible SSH Bruteforce"; flow:to_server,established; classtype:suspicious-login; threshold:type both, track by_src, count 10, seconds 10; sid:1000006; rev:2;)

---
  
### Outbound Curl request detection
alert http $HOME_NET any -> any any (msg:"Alert: Oubound request detected"; flow:to_server,established; http.user_agent; content:"curl/"; startswith; classtype:policy-violation; sid:1000007; rev:1;)

---
  
### Bash Reverse Shell detection
alert tcp any any -> $HOME_NET [4444,5555,6666,7777,8888,9999] (msg:"Alert: Possible Reverse Shell"; flow:established,to_server; classtype:bad-unknown; sid:1000008; rev:1;)

---
  
### DNS Exfiltration detection
alert dns $HOME_NET any -> any any (msg:"Alert: Potential DNS Tunneling/Exfiltration"; dns.query; pcre:"/[a-zA-Z0-9_-]{40,}\./"; classtype:trojan-activity; sid:1000009; rev:1;)

---
  
