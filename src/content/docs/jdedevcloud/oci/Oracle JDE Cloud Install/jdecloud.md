---
title: Oracle JDE Cloud Trial Edition Install
---


## Summary

!!!note ""
    Oracle Cloud JD Edwards EnterpriseOne Trial Edition provides the capability to test JDE in a real working environment consisting of a JDE Web, Orchestration and Enterprise Servers. This will allow you to run JDE interactive applications, reports, and orchestrations.

## References

!!!note "Oracle"
    - Oracle : [Oracle Cloud](https://www.oracle.com/au/cloud/)
    - Oracle Learning Library : [Deploying JD Edwards EnterpriseOne Trial Edition on Oracle Cloud Infrastructure](https://apexapps.oracle.com/pls/apex/f?p=44785:50:0:::50:P50_EVENT_ID,P50_COURSE_ID:6152,395)

## Prerequisites
- Oracle Cloud account (free tier accepted)
- SSH keys (can be created during VM provisioning)

## Launch JD Edwards EnterpriseOne Trial Edition

!!!note "Locate Oracle Cloud Marketplace"
    ![](<2025-03-20_10_53_02-Home _ Oracle Cloud Infrastructure.png>)

!!!note "Select All Applications"
    ![](<2025-03-20_10_53_23-Home _ Oracle Cloud Infrastructure.png>)


!!!note "Search "jd edwards""
    ![](<2025-03-18 13_11_40-All Applications _ Oracle Cloud Infrastructure.png>)

!!!note "Select JDE Edwards EnterpriseOne Trial Edition"
    ![](<2025-03-18 13_13_20-All Applications _ Oracle Cloud Infrastructure.png>)

!!!note "Launch VM Instance"
    ![](<2025-03-18 13_13_48-Marketplace _ Oracle Cloud Infrastructure.png>)

!!!note "Set VM Instance name"
    ![](<2025-03-18 13_14_21-Instances _ Oracle Cloud Infrastructure.png>)

!!!note "Review default VM Instance Configuration"
    ![](<2025-03-18 13_15_23-Instances _ Oracle Cloud Infrastructure.png>)

!!!note "Set ssh key"
    ![](<2025-03-18 13_14_55-Instances _ Oracle Cloud Infrastructure.png>)

!!!note "Create VM Instance"
    ![](<2025-03-18 13_15_34-Instances _ Oracle Cloud Infrastructure.png>)

!!!note "VM Instance is Running"
    ![](<2025-03-18 13_17_45-Instances _ Oracle Cloud Infrastructure.png>)

## First-Time Configuration

Connect to VM Instance. Use public ip address from the newly created VM

```bash
$ ssh -i ~/.ssh/id_ed25519_serge opc@152.69.178.250
```

Automated Update Process Started.
System invokes automated update script, wait until finished. System will reboot / disconnect automatically.

```bash
The keyboard will be DISABLED while yum update is running.
The server will reboot after yum update has completed.

Complete!

Yum update is complete.  System will reboot now

Connection to 152.69.178.250 closed by remote host.
Connection to 152.69.178.250 closed.
```

Connect to VM Instance. 

```bash
$ ssh -i ~/.ssh/id_ed25519_serge opc@152.69.178.250
```

System prompts for web server port and passwords. Accept default port and provide appropriate passwords.

```bash
=======================================
Trial Edition Configuration
=======================================
Server name (hostname): jde92
HTML Port [8080]:
------------------
Database System Password: ********
JDE User Password: ********
Weblogic Admin Password: ********
=====================
Configure Trial Edition? [Y]es or [N]o: Y
```

Automated Configuration Process Completed. Review Server URLs

```bash
=================================================
BACKGROUND CONFIGURATION PROCESSES HAVE COMPLETED
=================================================

The url for EnterpriseOne is:
        https://152.69.178.250:8080/jde/owhtml

The url for Server Manager is:
        https://152.69.178.250:8998/manage

The url for Studio is:
        https://152.69.178.250:7077/studio

Completed Trial Edition Configuration           Run Time: 12:06
```

## JDE Servers are ready

Review JDE URLs invoking jde-url script 

```bash
$ jde-url
```

Script Output 

```bash
The url for EnterpriseOne is:
        https://152.69.178.250:8080/jde/owhtml

The url for Server Manager is:
        https://152.69.178.250:8998/manage

The url for Studio is:
        https://152.69.178.250:7077/studio
```


Get JDE Servers status invoking jde-status script

```bash
$ jde-status

TRIAL EDITION SERVER STATUS
ORACLE DATABASE STATUS
Status:  listener is UP
Status:  database is UP
ENTERPRISE ONE STATUS
Status:  jdenet_n processes are UP
Status:  jdenet_k processes are UP
HTML DOMAIN STATUS
Status nodemanager     : Server is Running         ( PID#: 59149   |  Port#: 5556   |  Port Status: UP   )
Status jdeAdminServer  : Server is Running         ( PID#: 59497   |  Port#: 7005   |  Port Status: UP   )
Status html_server     : Server is Running         ( PID#: 60148   |  Port#: 8080   |  Port Status: UP   )
Status ais_server      : Server is Running         ( PID#: 60588   |  Port#: 7077   |  Port Status: UP   )
BIP DOMAIN STATUS
Status nodemanager     : Server is Running         ( PID#: 59369   |  Port#: 9556   |  Port Status: UP   )
Status AdminServer     : Server is Running         ( PID#: 59653   |  Port#: 9703   |  Port Status: UP   )
Status bi_server1      : Server is Running         ( PID#: 61068   |  Port#: 9705   |  Port Status: UP   )
SMC DOMAIN STATUS
Status nodemanager                                 : Server is Running         ( PID#: 55640   |  Port#: 5559   |  Port Status: UP   )
Status smcAdminServer                              : Server is Running         ( PID#: 56113   |  Port#: 9002   |  Port Status: UP   )
Status SMC_Server_EOne_ManagementConsole1_Console  : Server is Running         ( PID#: 56582   |  Port#: 8998   |  Port Status: UP   )
SMC AGENT STATUS
EnterpriseOne Server Agent:  Agent is Running
HTML & AIS Server Agent:     Agent is Running
```

Stop JDE Servers before restarting the OS, and start them again after rebooting:

```bash
$ jde-stop
```

Script Output

```
Stopping Trial Edition Services in Parallel
Stopping the Trial Edition Enterprise Server
Stopping the Trial Edition Database Server
Stopping the Trial Edition HTML Server
Stopping the Trial Edition SMC Server
Stopping the Trial Edition BIP Server
All Processes Have Been Shutdown.       Run Time: 05:18

```

Reboot VM

```bash
$ reboot now
```

Reconnect to VM and Start JDE Servers

```bash
$ jde-start
```

## Review JDE Servers

### JDE Web Server

Navigate to appropriate URL to review JDE Web Server

!!!note "Web Server"
    [https://jde.jde2.jdedev.cloud/jde/E1Menu.maf](https://jde.jde2.jdedev.cloud/jde/E1Menu.maf)

!!!note "Ignore privacy error and Navigate to URL"
    ![](<2025-03-18 13_46_06-Privacy error.png>)

!!!note "Enter JDE credentials"
    ![](<2025-03-18 13_46_43-JD Edwards.png>)
    ![](<2025-03-18 13_47_03-JD Edwards.png>)

!!!note "JDE Homepage"
    ![](<2025-03-18 13_49_40-JD Edwards.png>)
    ![](<2025-03-18 14_19_56-JD Edwards.png>)

!!!note "Start Batch Versions from Fast Path"
    ![](<2025-03-18 13_51_35-JD Edwards.png>)

!!!note "Select and submit Report Version"
    ![](2025-03-18%2014_20_13-Batch Versions - Work With Batch Versions - Available Versions.png)

!!!note "Review Submitted Jobs"
    ![](2025-03-18%2014_20_43-Submitted Job Search.png)

!!!note "Open Business Unit Report"
    ![](<2025-03-18 14_21_06-Business Unit Report.png>)

!!!note "Start Address Book Interactive Application from Fast Path"
    ![](2025-03-18%2014_21_15-Batch Versions - Work With Batch Versions - Available Versions.png)

!!!note "Select Address Book Record"
    ![](2025-03-18%2014_21_28-Work With Addresses.png)

!!!note "Review Address Book Record"
    ![](2025-03-18%2014_21_37-Address Book Revision.png)

!!!note "Start Sales Order Entry Interactive Application from Fast Path"
    ![](2025-03-18%2014_21_43-Work With Addresses.png)

!!!note "Copy a Sales Order"
    ![](2025-03-18%2014_21_57-Customer Service Inquiry.png)

!!!note "Review Sales Order Details"
    ![](2025-03-18%2014_22_11-Sales Order Detail Revisions.png)

!!!note "Locate New Order"
    ![](2025-03-18%2014_22_37-Customer Service Inquiry.png)

### JDE Server Manager Server

Navigate to appropriate URL to review JDE Server Manager

!!!note "Web Server"
    [https://manage.jde2.jdedev.cloud/manage/logon](https://manage.jde2.jdedev.cloud/manage/logon)

!!!note "Provide Server Manager credentials (jde_admin : default admin account)"
    ![](<2025-03-18 13_48_02-Server Manager for JD Edwards EnterpriseOne.png>)    

!!!note "All servers are running"
    ![](<2025-03-18 13_48_37-Managed Homes and Managed Instances.png>)

### JDE Orchestration Studio

Navigate to appropriate URL to review JDE Orchestration Studio

!!!note "Web Server"
    [https://orch.jde2.jdedev.cloud/studio/](https://orch.jde2.jdedev.cloud/studio/)

!!!note "Provide JDE Orchestrator credentials"
    ![](<2025-03-18 13_48_23-Orchestrator Studio Login.png>)    

!!!note "JDE Orchestrator is Running"
    ![](<2025-03-18 14_23_04-JDE Orchestrator Studio.png>)

## Destroy JD Edwards EnterpriseOne Trial Edition

!!!note "Locate VM Instance"
    ![](<2025-03-18 14_24_14-Instances _ Oracle Cloud Infrastructure.png>)

!!!note "Select Actions - Terminate"
    ![](<2025-03-18 14_24_20-Instances _ Oracle Cloud Infrastructure.png>)

!!!note "Enable : Permanently delete the attached boot volumes and I acknowledge that terminating these instances might result in data loss. Select Terminate"
    ![](<2025-03-18 14_24_26-Instances _ Oracle Cloud Infrastructure.png>)

!!!note "VM Instance terminated, Select Close"
    ![](<2025-03-18 14_26_04-Instances _ Oracle Cloud Infrastructure.png>)

## JD Edwards EnterpriseOne Trial Edition VM Costs

!!!note "Navigate to Cost analysis and enable Forecast"
    ![](<2025-03-18 14_25_47-Cost Analysis _ Oracle Cloud Infrastructure.png>)

!!!note "Compute Cost Forecast is in red box. ~ $3 AUD / day"
    ![](<2025-03-18 14_27_36-Cost Analysis _ Oracle Cloud Infrastructure.png>)
    ![](<2025-03-18 14_27_53-Cost Analysis _ Oracle Cloud Infrastructure.png>)




