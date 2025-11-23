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

📂 Project Structure

ddos_demo/
 ├── venv/
 ├── ddos_server.py
 └── README.md

🧰 Tech Stack
Backend

 1. Python 3.x – Core programming language

 2. Flask – Lightweight web framework for handling incoming HTTP requests

Security / Logic

 1. IP Rate Limiting – Custom Python logic for counting requests

 2. In-memory Data Structures (Dict, Lists) – To store IP request timestamps

 3. Blocking Mechanism – Temporary IP blacklist logic

Email Notifications

 1. smtplib (Python SMTP Library) – To send email alerts

 2. Gmail App Passwords – For secure authentication

Environment & Tools

 1. Virtual Environment (venv) – For package isolation

 2. Command Prompt / PowerShell – To run the project

 3. Windows 10 – Development and execution platform

⚙️ Installation & Setup

Follow the steps below to run the DDoS Detection System on your local machine:

1️⃣ Navigate to the Project Folder

  Use the command prompt to move into your project directory:

    cd C:\Users\LENOVO\ddos_demo

2️⃣ Open the Server Script

  Use Notepad to code Python server file:

    notepad ddos_server.py

3️⃣ Python code

from flask import Flask, request

import time

import smtplib

from email.mime.text import MIMEText

import atexit

app = Flask(__name__)

# Email alert settings

ADMIN_EMAIL = "4mh24ca@gmail.com"        # Your Gmail

APP_PASSWORD = "Password"                # 16 letter app password

ALERT_TO = "4mh24ca@gmail.com"           # Where alerts should be sent

# Rate limit settings

WINDOW = 10

BLOCK = 5

request_count = {}

blocked = {}

total_requests = {}

# ---------------------------
# Email sending function
# ---------------------------

def send_email_alert(ip, hits):

    try:
    
        subject = "⚠ ALERT: Suspicious Activity Detected"
        
        message = f"""
        
        🚨 DDoS Alert Triggered!

        Suspicious IP has been blocked:

        🔹 IP Address: {ip}
        
        🔹 Requests in last 10s: {hits}
        
        🔹 Time: {time.ctime()}

        Please check your DDoS protection server.
        
        """

        msg = MIMEText(message)
        
        msg["Subject"] = subject
        
        msg["From"] = ADMIN_EMAIL
        
        msg["To"] = ALERT_TO

        server = smtplib.SMTP("smtp.gmail.com", 587)
        
        server.starttls()
        
        server.login(ADMIN_EMAIL, APP_PASSWORD)
        
        server.sendmail(ADMIN_EMAIL, ALERT_TO, msg.as_string())
        
        server.quit()

        print(f"[EMAIL] Alert sent to {ALERT_TO} for IP {ip}")

    except Exception as e:
    
        print("[EMAIL ERROR]", e)

# ---------------------------
# Server shutdown summary
# ---------------------------

def show_summary():

    print("\n========== SERVER SUMMARY ==========")
    
    print(f"Total unique visitors: {len(total_requests)}")

    for ip, count in total_requests.items():
    
        print(f"IP: {ip} → Total Requests: {count}")

    print("\nBlocked IPs:")
    
    if blocked:

        for ip in blocked:
        
            print(f"❌ {ip} was blocked")
            
    else:
    
        print("No IPs were blocked.")
        
    print("====================================\n")

atexit.register(show_summary)


# ---------------------------
# Main server logic
# ---------------------------

@app.route("/")

def index():

    ip = request.remote_addr
    
    now = time.time()

    total_requests[ip] = total_requests.get(ip, 0) + 1

    if ip in blocked:
    
        print(f"[BLOCKED] Request from blocked IP {ip}")
        
        return "⛔ You are blocked!"

    request_count.setdefault(ip, [])
    
    request_count[ip].append(now)

    request_count[ip] = [t for t in request_count[ip] if now - t < WINDOW]
    
    hits = len(request_count[ip])

    print(f"[INFO] IP: {ip} | Requests in last 10 sec: {hits}")

    if hits > BLOCK:
    
        blocked[ip] = now
        
        print(f"[BLOCKED] {ip} blocked! (Hits: {hits})")

        # SEND EMAIL ALERT HERE
        
        send_email_alert(ip, hits)

        return "🚫 Blocked due to too many requests!"

    return f"Welcome! Your IP: {ip} | Recent hits: {hits}"


if __name__ == "__main__":

    app.run(host="0.0.0.0", port=5000)

Make any necessary updates to email settings or configurations inside the file.

4️⃣ Activate the Virtual Environment

  Start the Python virtual environment:

     venv\Scripts\activate

5️⃣ Run the DDoS Detection Server

  Start the Flask-based DDoS detection system:

     python ddos_server.py

📊 Output Example

[INFO] IP: 10.227.9.129 | Requests in last 10 sec: 5
[BLOCKED] 10.227.9.129 blocked for too many requests! (Hits: 6)
[EMAIL SENT] Alert sent to admin

📬 Author

Ranjith M

B.E – Artificial Intelligence

Maharaja Institute of Technology Mysore
