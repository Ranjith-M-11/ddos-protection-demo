# ddos-protection-demo
A simple Python Flask-based DDoS Detection System that monitors incoming requests, detects abnormal traffic, blocks attackers, and sends email alerts. It logs IP activity in real time, applies rate limiting, and demonstrates basic cloud security concepts like request monitoring and attacker blocking.
🛡️ DDoS Detection & Prevention System

A lightweight Python–Flask based project that detects abnormal traffic, blocks attackers, and sends email alerts when suspicious activity is detected.
Designed as a simple demo model for cloud security and rate-limiting concepts.

🚀 Features

✔ Real-time request monitoring

✔ IP-based rate limiting

✔ Automatic attacker blocking

✔ Email alert when an IP is blocked

✔ Console logging with IP, hits, and status

✔ Easy to run on Windows (no Linux required)

🧠 How It Works

The server records how many times each IP sends a request within a time window.

If an IP exceeds the allowed limit, it is marked as blocked.

A notification email is sent instantly to the admin.

Blocked IPs are prevented from accessing the server for a cooldown period.

📂 Project St
