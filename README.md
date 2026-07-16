# 🛡️ SOC Home Lab

> My journey of building a Security Operations Center (SOC) from scratch to understand how real Blue Teams detect, investigate, and respond to cyber attacks.

---

## 📖 Project Overview

I built this SOC Home Lab to gain practical experience with how a Security Operations Center (SOC) works by creating my own monitoring environment from scratch.

The lab was built on VMware Workstation using multiple virtual machines, including Ubuntu Server, Windows 10, and Kali Linux. On the Ubuntu server, I deployed the ELK Stack (Elasticsearch, Logstash, and Kibana) to collect, store, and visualize security logs. I also configured Fleet and enrolled the Windows machine as an endpoint so its logs could be monitored centrally.

After setting up the environment, I simulated different attack scenarios against the Windows machine, including Credential Stuffing, DNS Tunnelling, and PowerShell Exploitation. I created custom detection rules, generated alerts, analyzed the collected logs in Kibana, and verified whether each alert was a true positive.

While building the lab, I also dealt with setup issues such as service failures, agent enrollment problems, log collection issues, and detection rule tuning. Solving these problems helped me understand how the different components of a SOC work together.

This repository contains the complete setup process, architecture, attack simulations, detection rules, investigation steps, screenshots, challenges, and the lessons I learned while building this project.

## 🎯 Project Goals

- Build a virtual SOC environment using VMware Workstation.
- Set up the ELK Stack (Elasticsearch, Logstash, and Kibana) on an Ubuntu Server.
- Connect a Windows endpoint using Elastic Agent and Fleet for centralized log collection.
- Collect and analyze Windows Security and PowerShell logs.
- Simulate attack scenarios such as Credential Stuffing, DNS Tunnelling, and PowerShell Exploitation.
- Create and test custom detection rules to generate meaningful security alerts.
- Investigate alerts in Kibana and validate whether they were true or false positives.
- Improve my understanding of SIEM, Detection Engineering, Threat Hunting, and incident investigation through hands-on practice.
- Document the complete build process, challenges, and solutions so the project can be reproduced by others.

## 🏗️ How My SOC Home Lab Works

Before creating detection rules or investigating alerts, I first needed a way to collect security logs from a Windows machine. The diagram below shows how information travels through my lab—from an attack being performed to an alert appearing in Kibana.

```text
                 ┌──────────────────┐
                 │   Kali Linux     │
                 │  (Attacker)      │
                 └────────┬─────────┘
                          │
                Simulates Cyber Attacks
                          │
                          ▼
                 ┌──────────────────┐
                 │   Windows 10     │
                 │ (Monitored PC)   │
                 └────────┬─────────┘
                          │
      Elastic Agent collects Windows logs
                          │
                          ▼
                 ┌──────────────────┐
                 │  Fleet Server    │
                 │ (Ubuntu Server)  │
                 └────────┬─────────┘
                          │
          Sends logs to the ELK Stack
                          │
                          ▼
                 ┌──────────────────┐
                 │ Elasticsearch    │
                 │  Stores the Logs │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │     Kibana       │
                 │ Search & Analyze │
                 └────────┬─────────┘
                          │
          Detection Rules Monitor Logs
                          │
                          ▼
                 ┌──────────────────┐
                 │ Security Alerts  │
                 │  Investigation   │
                 └──────────────────┘
```

### What Happens Behind the Scenes?

1. I launch an attack from the **Kali Linux** machine.
2. The attack targets the **Windows 10** machine.
3. The **Elastic Agent** running on Windows collects security events and system logs.
4. Those logs are sent to the **Fleet Server** and then stored in **Elasticsearch**.
5. **Kibana** displays the collected logs and continuously checks them against my custom detection rules.
6. If a log matches one of my detection rules, Kibana generates a security alert.
7. Finally, I investigate the alert to verify whether it represents a real attack or a false positive.

## 🔄 Project Workflow

The diagram below shows the complete workflow I followed while building and testing my SOC Home Lab.

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ 1. Prepare the Virtual Lab                                                  │
│                                                                             │
│ Platform : VMware Workstation Pro                                           │
│                                                                             │
│ • Installed VMware Workstation Pro                                          │
│ • Created Ubuntu Server, Windows 10 and Kali Linux VMs                      │
│ • Allocated CPU, RAM, Storage and configured virtual networking             │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ 2. Deploy the ELK Stack                                                     │
│                                                                             │
│ Machine : Ubuntu Server                                                     │
│                                                                             │
│ • Installed Elasticsearch                                                   │
│ • Installed Logstash                                                        │
│ • Installed Kibana                                                          │
│ • Verified all ELK services were running                                    │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ 3. Configure Fleet Server                                                   │
│                                                                             │
│ Machine : Ubuntu Server                                                     │
│                                                                             │
│ • Enabled Fleet in Kibana                                                   │
│ • Configured Fleet Server                                                   │
│ • Created an Agent Policy                                                   │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ 4. Connect the Windows Endpoint                                             │
│                                                                             │
│ Machine : Windows 10                                                        │
│                                                                             │
│ • Installed Elastic Agent                                                   │
│ • Enrolled the endpoint into Fleet                                          │
│ • Verified the agent status                                                 │
│ • Confirmed logs were reaching Elasticsearch                                │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ 5. Collect Windows Security Logs                                            │
│                                                                             │
│ Source : Windows 10                                                         │
│                                                                             │
│ • Windows Security Events                                                   │
│ • PowerShell Logs                                                           │
│ • Authentication Events                                                     │
│ • Process & System Activity                                                 │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ 6. Simulate Attack Scenarios                                                │
│                                                                             │
│ Attacker : Kali Linux                                                       │
│ Target   : Windows 10                                                       │
│                                                                             │
│ • Credential Stuffing                                                       │
│ • DNS Tunnelling                                                            │
│ • PowerShell Exploitation                                                   │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ 7. Create Detection Rules                                                   │
│                                                                             │
│ Platform : Kibana Security                                                  │
│                                                                             │
│ • Created custom detection rules                                            │
│ • Configured rule conditions                                                │
│ • Tested each rule against simulated attacks                                │
│ • Tuned detections to reduce false positives                                │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ 8. Investigate Security Alerts                                              │
│                                                                             │
│ Platform : Kibana                                                           │
│                                                                             │
│ • Reviewed generated alerts                                                 │
│ • Investigated related logs                                                 │
│ • Validated detection accuracy                                              │
│ • Identified true and false positives                                       │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│ 9. Document the Project                                                     │
│                                                                             │
│ • Captured screenshots                                                      │
│ • Recorded configurations                                                   │
│ • Documented challenges and solutions                                       │
│ • Summarized key learnings                                                  │
└─────────────────────────────────────────────────────────────────────────────┘
```
## 📈 Results

After completing the lab, I was able to successfully build a functional SOC environment capable of collecting logs, detecting suspicious activities, and generating security alerts.

### Key Outcomes

- Built a multi-machine virtual lab using VMware Workstation.
- Successfully deployed the ELK Stack.
- Connected the Windows endpoint using Elastic Agent.
- Collected Windows Event Logs in Kibana.
- Created and tested custom detection rules.
- Simulated multiple attack scenarios to validate detections.
- Investigated generated alerts and verified detection accuracy.
- Documented the complete setup and troubleshooting process.

> Screenshots of the dashboard, detections, and attack simulations are included throughout this repository.

## ⚠️ Challenges & Troubleshooting

Building the lab involved several issues that required troubleshooting before the environment became stable.

Some of the challenges I encountered include:

- ELK services failing to start correctly.
- Fleet Server installation and enrollment issues.
- Elastic Agent showing as offline.
- Windows endpoint not sending logs.
- Detection rules not triggering as expected.
- Incorrect field mappings while creating rules.
- Network communication issues between virtual machines.
- High memory usage while running multiple virtual machines.

Each issue helped me understand the platform better and improved my troubleshooting approach. The complete solutions are documented in the `docs` folder.

## 📚 What I Learned

This project helped me move beyond theory and understand how different parts of a Security Operations Center work together.

Through this lab, I gained practical experience with:

- Building a virtual enterprise lab.
- Deploying and configuring the ELK Stack.
- Managing Elastic Agents using Fleet.
- Collecting and analyzing Windows Event Logs.
- Writing and testing detection rules.
- Investigating security alerts in Kibana.
- Understanding the complete detection lifecycle.
- Troubleshooting real deployment issues.
- Documenting technical projects in a structured way.

More importantly, I learned how data flows from an endpoint to the SIEM and how that data can be used to detect suspicious activity.

## 📂 Repository Structure

```
SOC-Home-Lab/
│
├── README.md
├── assets/
├── docs/
│   ├── 01-Lab-Setup.md
│   ├── 02-ELK-Installation.md
│   ├── 03-Fleet-Configuration.md
│   ├── 04-Log-Collection.md
│   ├── 05-Detection-Rules.md
│   ├── 06-Attack-Simulations.md
│   ├── 07-Alert-Investigation.md
│   ├── 08-Challenges.md
│   └── 09-Lessons-Learned.md
│
└── report/
```

The `docs` folder contains the detailed implementation of each stage, while this README provides a high-level overview of the entire project.

## 🚀 Future Improvements

This lab will continue to evolve as I learn more about Blue Team operations and Detection Engineering.

Some improvements I plan to make include:

- Add more Windows endpoints.
- Integrate Sysmon for detailed endpoint telemetry.
- Simulate additional attack techniques based on MITRE ATT&CK.
- Develop more advanced detection rules.
- Perform threat hunting using collected logs.
- Integrate Threat Intelligence feeds.
- Build automated alerting and reporting.
- Add more attack simulations using Atomic Red Team.
