## Performance Note

Since the implemented rules are signature-based, certain legitimate administrative activities (such as authorized Nmap scans or penetration testing) may trigger alerts. Proper tuning and whitelisting should be applied before deployment in production environments.

During testing, Suricata successfully detected the simulated attacks without noticeable impact on system responsiveness. However, production deployments require adequate CPU, memory, and storage resources, especially when Deep Packet Inspection is enabled.
