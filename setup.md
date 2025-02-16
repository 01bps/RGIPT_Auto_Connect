# Kindly go through the steps to locally setup the script on your device.
___
## 1. Install Dependencies  

Run the following command to install the requirements:  

```bash
pip install -r requirements.txt
```
## 2. Find Your Network's Login URL and Payload
```
2.1 Connect to your WiFi network manually.
2.2 Open the login page in a browser (http://192.168.x.x:8090/httpclient.html).
2.3 Inspect the login form (Right-click → Inspect → Network Tab).
2.4 Find the POST request URL inside the Network tab(should look like http://192.168.x.x:8090/login.xml).
2.5 Copy the form data parameters (like username, password, a, etc.).
```
## 3. Edit ```wifi_auto_login.py``` file
Modify the ```def wifi_login()``` function to match your payload parameters.
i.e:
```
{
    "login_url": "http://192.168.100.1:8090/login.xml",
    "username": "your_username",
    "password": "your_password",
    "payload_params": {
        "mode": "191",
        "username": "your_username",
        "password": "your_password",
        "a": "dynamic",
        "producttype": "0"
    },
    "db_name": "wifi_log.db"
}
```
Note: a → Keep "dynamic" if the value changes on every login attempt.

## 4. Run the Script
Run the Python script to log in automatically:
```
python wifi_auto_login.py
```

## 5. Automate WiFi Login on System Boot:

#### **🔹 Windows (Task Scheduler)**
5.1. Open **Task Scheduler** → **Create Basic Task**.  
5.2. Set **Trigger** → **At Startup**.  
5.3. Set **Action** → **Start a Program** → Browse for `python.exe`.  
5.4. In **Arguments**, enter:  
   ```text
   "C:\path\to\wifi_auto_login.py"
   ```
5.5. Save and enable the task.  

---
#### **🔹 Linux (Crontab)**
5.1. Open terminal and run:  
   ```bash
   crontab -e
   ```
5.2. Add this line at the end:  
   ```bash
   @reboot python3 /path/to/wifi_auto_login.py
   ```
5.3. Save and exit.  
---

#### **🔹 macOS (Launch Agents)**
5.1. Create a `.plist` file in `~/Library/LaunchAgents/`:  
   ```bash
   nano ~/Library/LaunchAgents/wifi_auto_login.plist
   ```
5.2. Add this content:  
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist version="1.0">
     <dict>
       <key>Label</key>
       <string>wifi_auto_login</string>
       <key>ProgramArguments</key>
       <array>
         <string>/usr/bin/python3</string>
         <string>/path/to/wifi_auto_login.py</string>
       </array>
       <key>RunAtLoad</key>
       <true/>
     </dict>
   </plist>
   ```
5.3. Save and enable it:  
   ```bash
   launchctl load ~/Library/LaunchAgents/wifi_auto_login.plist
   ```

We have succesfully setup the script now the wifi or LAN will get connected **automatically on system startup**!

