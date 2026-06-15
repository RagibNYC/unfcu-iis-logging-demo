UNFCU Banking System - IIS Logging & Incident Response
Professional IT incident response demonstration showcasing infrastructure setup, monitoring, log analysis, and production recovery.
_________________________________________________________________________________________________________________________________________________________________________

Created an IIS-based incident response demonstration showing a production database 
failure, systematic diagnosis using logs and monitoring, identification of the root 
cause (DNS misconfiguration), and complete recovery—demonstrating the infrastructure 
and troubleshooting skills required for IT Analyst roles at credit unions. 

_________________________________________________________________________________________________________________________________________________________________________
📋 Project Objective

Demonstrate core IT infrastructure skills:
System setup and configuration
Real-time monitoring
Log analysis and pattern recognition
Incident diagnosis and root cause analysis
Production recovery procedures
Professional documentation

_________________________________________________________________________________________________________________________________________________________________________

Programs Used: 
Cloud Platform: Microsoft Azure 
Microsft Remote Desktop Protocol
Operating System: Windows Server 2022
Web Server: IIS 10.0
Monitoring Tool: Performance Monitor

_________________________________________________________________________________________________________________________________________________________________________

🎯 What This Project Shows

Created 2 Programs:
1. UNFCU Banking Demo Website 

UNFCU-themed lending system interface
Demonstrates different HTTP error codes (200, 404, 403, 500, 503)
Real-time request logging

2. UNFCU Database Connection Test Page

Interactive diagnostic tool
Simulates database connectivity failures
Shows error analysis and recovery process	

_________________________________________________________________________________________________________________________________________________________________________


📂 The Complete Incident Story

🏗️ Setup (What Was Built)


UNFCU demo website on IIS with automatic HTTP logging
Website links simulate different error codes (200, 404, 403, 500, 503)
Each request logged with timestamp, HTTP method, resource, response code, and response time
Members can click buttons to test different API endpoints
Each request is automatically logged by IIS in W3C format

_________________________________________________________________________________________________________________________________________________________________________


🚨 The Problem (What Went Wrong)

Production lending system loses database connectivity

Impact:
✗ Members cannot submit new loan applications
✗ Loan origination system is unavailable
✗ Credit check system unavailable
✗ Mortgage processing stopped
✗ Balance inquiries failing
✗ 15 member transactions delayed

System Status: 503 Service Unavailable
The database became unreachable. The system returned 503 Service Unavailable. Loan processing stopped completely.

🔍 Root Cause Analysis

What Went Wrong:
Infrastructure domain migration changed domain names, but the application configuration file wasn't updated.

OLD (Broken):
  Hostname: db-lending-prod-01.unfcu.internal:5432
  Status: ✗ Does not exist (DNS cannot resolve)

NEW (Correct):
  Hostname: db-lending-prod-01.unfcu.local:5432
  Status: ✓ Exists (DNS resolves correctly)

Why It Happened:
Infrastructure team migrated from .internal to .local domain
Change was not communicated to all application teams
Configuration file retained outdated hostname
Deployment validation checks were not in place

_________________________________________________________________________________________________________________________________________________________________________

🔧 Diagnosis Process

Step 1: Detection (~1 minute)

Automated monitoring alert triggered
Database connection timeout detected
System monitoring identified the issue immediately

Step 2: Log Analysis (~2 minutes)

IIS logs showed repeated connection attempts with ECONNREFUSED errors:
Server: db-lending-prod-01.unfcu.internal:5432
Error: ECONNREFUSED (connection refused)
Status: All 3 retry attempts failed with SAME error

Key Finding: All 3 retry attempts failed with the exact same error. This indicates a systematic problem, not a transient network issue.

Step 3: Performance Monitoring

During the Incident:
CPU: 60% (baseline 5-10%)
Memory: 5.5GB (from 7GB)
Retry logic consuming significant resources
Connection pool exhausted

Performance Monitor Graph: Shows clear spikes in CPU and memory usage during retry attempts.

Step 4: Root Cause Testing

Hypothesis Testing:
Is the database server down?
✗ No - different error would occur

Is the hostname incorrect or DNS cannot resolve it?
✓ YES - Testing confirms this
ping db-lending-prod-01.unfcu.internal → FAILS (hostname doesn't exist)
ping db-lending-prod-01.unfcu.local → SUCCEEDS (correct hostname)

Is there a network connectivity issue?
✗ No - network is fine

Is the firewall blocking port 5432?
✗ No - if firewall was blocking, we'd get different error

Conclusion: 🎯 ROOT CAUSE IDENTIFIED

The application configuration file references an incorrect database hostname. The .internal domain no longer exists after the infrastructure migration. The correct hostname is .local.

_________________________________________________________________________________________________________________________________________________________________________


💻 Technologies Used

TechnologyUsageMicrosoft AzureCloud infrastructure and VM hostingWindows Server 2022Operating systemIIS 10.0Web server and HTTP request handlingWindows RDPRemote connection to VMPerformance MonitorCPU and Memory trackingIIS Native LoggingAutomatic HTTP request logging (W3C format)HTML5/CSS3/JavaScriptWebsite and diagnostic tools


📊 Project Statistics

Infrastructure:

Cloud Platform: Microsoft Azure
Operating System: Windows Server 2022
Web Server: IIS 10.0
Monitoring Tool: Performance Monitor


Incident Details:

Root Cause: DNS hostname misconfiguration (.internal vs .local)
Resolution Time: 5 minutes (detection → diagnosis → fix → recovery)
Configuration Changes: 1 line in web.config
Member Impact: 15 loan applications delayed


Demonstration Scope:

HTTP Status Codes: 5 different error scenarios (200, 404, 403, 500, 503)
Screenshots: 16 real incident evidence images
Documentation: Complete professional documentation
Code: 7 HTML pages demonstrating different scenarios
Database Failure Simulation: Complete with error logs and recovery
