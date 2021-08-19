# Adjust_Test_project

ASSIGNMENT 1:
================

Please write a simple CLI application in the language of your choice that does the following: Print the numbers from 1 to 10 in random order to the terminal. Please provide a README, that explains detailed how to run the program on MacOS and Linux.

(I could test on linux environment as I don’t have setup of Mac OS here)
How to work on Linux:
1. Create a python file :
2. $ vi randomnumber.py
3. write the code in editor and save it
4. run the python file and get the result on the terminal
$ python randomnumber.py

**********************************************************************************************************************************************************************************
ASSIGNMENT 2:
=================

Imagine a server with the following specs:
4 times Intel(R) Xeon(R) CPU E7-4830 v4 @ 2.00GHz
64GB of ram
2 tb HDD disk space
2 x 10Gbit/s nics
The server is used for SSL offloading and proxies around 25000 requests per second. Please let us know which metrics are interesting to monitor in that specific case and how would you do that? What are the challenges of monitoring this?


Solution:
============

Metrics to monitor:


Sever running state: 
====================

CPU:
=======
1) CPU load

2) CPU temperature(SSL-Offloading is CPU sensitive process)

3) Increase in file descriptor (Slower responses and higher wait time will cause high FD’s on server)

Network:
=========
1) TCP Open Connections (TCP Connections open (Internet<->Proxy<->Backend)
2) Throughput Monitoring.
3) Network traffic on NIC.
4) ethtool -S (should look for values with “drop”, “buffer”, “miss”, etc in the label)
5) Monitoring /proc/softirqs (give you an idea of how your network receive (NET_RX) processing is currently distributed across your CPUs)
6) Check hardware interrupt stats /proc/interrupts.
7) IP protocol statistics /proc/net/snmp + /proc/net/netstat.
8) UDP protocol statistics /proc/net/snmp + /proc/net/udp


Application Monitoring:
=======================
1) SSL Certificate Validation status.

2) Http request/response status

Memory :
========
1) Memory Free ( (Free memory on the server)

2) IO read/write

3) SSL-Offloading Proxy Server Process Aliveness (With Nagios/Zabbix etc )


How to monitor metrics?
================================

Method 1 :
===========
We can use either nagios or zabbix etc monitoring tools

Here we can use nagios/zabbix for monitoring where we can monitor below metrics:

1) CPU load (SSL-Offloading is CPU sensitive process)

2) Increase in file descriptor (Slower responses and higher wait time will cause high FD’s on server)

3) TCP Open Connections (TCP Connections open (Internet<->Proxy<->Backend) )

4) SSL Certificate Expiry on Proxy Server

5) Memory Free (Free memory on the server)

6) SSL-Offloading Proxy Server Process Aliveness (With Nagios )




Method 2: 
===========

I created a custom shell script to monitor each metric and send warning when metric value over expected,then add a cron job in the server to run.
How to run script gcmonitor.sh:
1) Create a file 
2) vi gcmonitor.sh
3) write the code in editor and save it
4) Change the permission to executable 
  chmod 755 gcmonitor.sh
5) Execute the script
##############################################################################################

Challenges of monitoring:
============================

1) To figure out important metrics which needs to be monitored.

2) Metrics monitoring tool should not effect the server's performance. 
   While using script method to monitor,logging the history performance value is very time consuming(read and write file) if file size if big.
  
3) To try and Set proper trigger warning value to minimize false alarm.

4) Internet Facing Firewall Monitoring : Catch Security Threats.

5) Monitoring of Monitoring and HA of monitoring itself : Its always challenge.

6) In some cases, the application is not compatible at all with SSL offloading. Challenge in differentiate of those clients?

