# SOC-Security-Operations-Center-lab

## 📑 Table of Contents
* [1. Project Overview](#1-project-overview)
* [2. Network Architecture](#2-network-architecture)
* [3. Skills & Tools Demonstrated](#3-skills--tools-demonstrated)
* [4. Environment Setup & Troubleshooting](#4-environment-setup--troubleshooting)
* [5. Vulnerability Management & Analysis](#5-vulnerability-management--analysis)
* [6. Incident Response Simulations](#6-incident-response-simulations)
* [7. Lessons Learned & Hardening](#7-lessons-learned--hardening)

---

## 1. Project Overview
This lab simulates a real-world enterprise environment using a **Wazuh SIEM**, a **pfSense firewall**, and a monitored **Windows 11 endpoint**. [cite_start]The goal is to establish a secure monitoring pipeline, identify system vulnerabilities, and simulate real-world attacks to test detection rules.

## 2. Network Architecture
The lab is built within a virtualized network using **VirtualBox**. Traffic is segmented by pfSense to ensure the victim machine is isolated from the host while remaining reachable by the Wazuh manager.

```mermaid
graph TD
    subgraph "External Access"
        Host[Physical Host Machine]
    end

    subgraph "Virtual Lab (VirtualBox)"
        WAN[NAT Network]
        FW[pfSense Firewall]
        
        subgraph "Internal Network (10.0.0.0/24)"
            Wazuh[Wazuh Manager: 10.0.0.101]
            Win[Windows 11 Victim: 10.0.0.100]
        end
    end

    Host --> WAN
    WAN --> FW
    FW --> Wazuh
    FW --> Win
    Win -- "Security Logs" --> Wazuh
