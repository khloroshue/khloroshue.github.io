---
layout: writeup
title: "HTB Writeup - Doctor"
date: 2020-09-27
---

# HackTheBox - Doctor - 10.10.10.209
#### 27/09/2020 13:44

## Initial Enumeration

The first bit of enumeration I did involved an initial nmap scan - checking all 65535 ports via the folowing syntax:

```shell
sudo nmap -p- -T4 -oN doctor.nmap.allports 10.10.10.209
```

#### *Contents of doctor.nmap.allports*
```shell
Starting Nmap 7.80 ( https://nmap.org ) at 2020-09-27 13:40 EDT
Nmap scan report for 10.10.10.209
Host is up (0.030s latency).
Not shown: 65532 filtered ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
8089/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 114.73 seconds
```

The initial allports scan shows that this box has three ports open; 22, 80, and 8089. I next ran another nmap script, this time using the `-sC` flag to run all scripts against the services, and then `-sV` flag to do version enumeration on the services.

```shell
sudo nmap -sC -sV -p22,80,8089 -oN doctor.nmap.openports
```

#### *Contents of doctor.nmap.openports*
```shell
# Nmap 7.80 scan initiated Sun Sep 27 13:43:49 2020 as: nmap -sC -sV -p22,80,8089 -oN doctor.nmap.openports 10.10.10.209
Nmap scan report for 10.10.10.209
Host is up (0.052s latency).

PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http     Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Doctor
8089/tcp open  ssl/http Splunkd httpd
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Splunkd
|_http-title: splunkd
| ssl-cert: Subject: commonName=SplunkServerDefaultCert/organizationName=SplunkUser
| Not valid before: 2020-09-06T15:57:27
|_Not valid after:  2023-09-06T15:57:27
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Sep 27 13:44:29 2020 -- 1 IP address (1 host up) scanned in 40.10 seconds
```

The first thing I noticed in this list was the output from the `http-robots.txt` script - it showed that there is one disallowed entry. Unfortunately, it shows that the disallowed entry is `/` - so it's not particularly helpful. 

## Webpage Enumeration

Opened the webpage for the service running on the odd web port `8089`. 
![afbb4b55685342200f04301a86130cd3.png](../assets/images/writeups/d342b1d4cdd24286830cdabfa3936bcb)
Opening all of the different links required a basic HTTP Auth - tried common culprits `admin/admin`, `admin/password`, `splunk/splunk`, and `admin/changeme` - which is documented to be the standard admin password for a fresh Splunk instance -- none of these worked.




