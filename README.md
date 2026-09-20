# SSH Logs Security Monitoring Dashboard (Splunk)

A Splunk Enterprise dashboard that monitors and analyzes SSH authentication logs in real time — detecting brute force attacks, tracking failed/successful logins, and visualizing attacker geo-location.


## 📌 Abstract

SSH is one of the most common protocols for secure remote server access — and also one of the most frequently targeted by brute force and unauthorized login attempts. This project uses **Splunk Enterprise** to collect, process, and visualize SSH authentication log data, giving security teams centralized visibility into login activity, suspicious IPs, and attacker origins.

## 🎯 Objectives

- Monitor SSH authentication activity
- Detect brute force attacks
- Identify suspicious IP addresses
- Analyze login trends over time
- Visualize attacker geo-location
- Provide a centralized security dashboard

## 🛠️ Tools & Technologies

| Category | Details |
|---|---|
| Platform | Splunk Enterprise |
| Log Format | JSON SSH logs |
| Server Environment | LinuxServer |
| Search Language | SPL (Search Processing Language) |
| Visualizations | Single Value Panels, Bar Charts, Statistics Tables, Choropleth Map |

## 🧪 Lab Setup

- **Log Source:** `ssh_logs.json`
- **Host:** `LinuxServer`
- **Sourcetype:** `_json`
- **Shared Time Picker Token:** `time_range`

All panels use a common Time Range filter for consistency.

## 📊 Dashboard Panels

### Authentication Overview

**1. Total SSH Events**
```spl
source="ssh_logs.json" host="LinuxServer" sourcetype="_json"
| stats count AS "Total SSH Events"
```

**2. Successful Logins**
```spl
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Successful SSH Login"
| stats count AS "Successful Logins"
```

**3. Failed Logins**
```spl
source="ssh_logs.json" host="LinuxServer" sourcetype="_json" event_type="Failed SSH Login"
| stats count AS "Failed Login"
```

**4. Invalid User Attempts**
```spl
index=auth "sshd" "invalid user"
| stats count AS "Invalid User Attempts"
```

### Login Activity Trends

**1. Failed Logins by Username (Bar Chart)**

Shows which usernames are most targeted.
```spl
source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Failed SSH Login"
| top username
```

**2. Possible Brute Force by IP (Statistics Table)**

Identifies IP addresses with multiple failed attempts. A high count from a single IP may indicate a brute force attack.
```spl
source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| top id.orig_h
```


**3. Brute Force Attack Geo-Location**

Visualizes attacker IP addresses by country using geo-location mapping. `iplocation` converts IP to country, and `geom` renders the data on a world map. Darker color indicates higher attack frequency.

```spl
source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| table id.orig_h
| iplocation id.orig_h
| stats count by Country
| geom geo_countries featureIdField="Country"
```


## ✨ Key Features

- Centralized SSH monitoring
- Real-time attack detection
- Brute force identification
- Geographic attack visualization
- Interactive time filtering

## 📈 Results & Findings

- Multiple failed login attempts detected from specific IP addresses
- Certain usernames were targeted frequently
- Geo-location analysis showed attack sources from multiple countries
- Possible brute force patterns were identified

## ✅ Conclusion

The **SSH Logs Dashboard** successfully monitors and analyzes SSH authentication activity using Splunk Enterprise. It helps security teams detect brute force attacks, monitor login trends, and identify attacker locations — improving overall system security visibility and enabling proactive threat detection.

## 📂 Repository Structure

```
.
├── README.md
└── screenshots/
    ├── dashboard-overview.jpg
    ├── brute-force-ip-table.png
    └── geo-location-map.jpg
```
