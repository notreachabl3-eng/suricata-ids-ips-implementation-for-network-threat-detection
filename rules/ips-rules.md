# Suricata custome rule file

### ICMP Ping Prevention
drop icmp any any -> $HOME_NET any (msg:"Dropped: ICMP flood detected"; itype:8; classtype:icmp-event; threshold:type both, track by_dst, count 3, seconds 10; sid:1000001; rev:1;)

---

### Nmap Scan Prevention
drop tcp any any -> $HOME_NET any (msg:"Dropped: Nmap Stealth Scan Detected"; flags:S; window:1024; flow:to_server; classtype:network-scan; threshold: type both, track by_src, count 5, seconds 20; sid:1000002; rev:2;)

---

### Web enumeration Prevention
drop http any any -> $HOME_NET any (msg:"Dropped: Possible Web Enumeration"; flow:to_server,established; http.user_agent; pcre:"/(gobuster|Mozilla/4.0|Gecko|Fuzz)/i"; classtype:web-application-attack; threshold: type both, track by_src, count 10, seconds 20; sid:1000003; rev:1;)

---

### HTTP Brute Force Prevention
drop http any any -> $HOME_NET any (msg:"Dropped: Possible HTTP Brute Force"; http.method; content:"POST"; http.uri; content:"login"; classtype:suspicious-login; threshold: type both, track by_src, count 5, seconds 10; sid:1000004; rev:1;)

---

### FTP Bruteforce Prevention
drop ftp any any -> $HOME_NET 21 (msg:"Dropped: Possible FTP Bruteforce"; flow:to_server,established; classtype:suspicious-login; threshold: type both, track by_src, count 5, seconds 10; sid:1000005; rev:1;)

---

### SSH Bruteforce Prevention
drop ssh any any -> $HOME_NET 22 (msg:"Dropped: Possible SSH Bruteforce"; flow:to_server,established; classtype:suspicious-login; threshold:type both, track by_src, count 5, seconds 10; sid:1000006; rev:1;)

---

### Outbound Curl request Prevention
drop http $HOME_NET any -> any any (msg:"Dropped: Oubound request detected"; flow:to_server,established; http.user_agent; content:"curl/"; startswith; classtype:policy-violation; sid:1000007; rev:1;)

---

### Bash Reverse Shell Prevention
drop tcp any any -> $HOME_NET [4444,5555,6666,7777,8888,9999] (msg:"Dropped: Possible Reverse Shell"; flow:established,to_server; classtype:bad-unknown; sid:1000008; rev:1;)

---

### DNS Exfiltration Prevention
drop dns $HOME_NET any -> any any (msg:"Dropped: Potential DNS Tunneling/Exfiltration"; dns.query; pcre:"/[a-zA-Z0-9_-]{40,}\./"; classtype:trojan-activity; sid:1000009; rev:1;)

---
