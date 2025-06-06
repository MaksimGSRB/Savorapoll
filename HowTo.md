README/HOWTO ON SAVORAPROJECT



# Savorapoll Project Guide


-------------------------------------------

### Uncommon Keywords and abbreviations:

Droplet = A linux based virtual machine, a term unique to and coined by DigitalOcean 

DOcean = DigitalOcean, a cloud computing provider

--------------------------------------------


## *Section 1: Introduction*

This document is an overview to the Savorapoll project, the list of the sections does not reflect the exact order of tasks done to create Savora.
It is however roughly in the order that everything was created. This guide is intended for myself in the future so I can go back and see exactly
what I have done and how because honestly I would forget. 

For everyone else: I hope this guide is clear enough so that people with moderate 
or possibly even a basic understanding of WebDev Languages (HTML, CSS, JavaScript) and Linux CLI can follow it. 

This guide will have a mix of dot points and indepth explanation for where seems necessary. 



## *Section 2: Basic Deployment*


### 2.1 IaaS/Cloud Initialisation 

- Using DigitalOcean
- Created project: SavoraProject
- Initialised one "Droplet" under "SavoraProject"
- Droplet name "SavoraHostname"
- The following are the droplets specs (note that DigitalOcean are not very specific when it comes to the exact specs)
  	- CPU:
  	  	- vCPUs = 1
  	  	- CPU Type = "Regular"  	  
  	- Memory:
  		- 1 GB
  	- Data Transfer:
  	  	- 1 TB Max
  	- OS
  	  	- Ubuntu 24.10 x64
- Evaluated chosen specs based of a TCO done previously; sufficient for now
- current costs $6 AUD monthly, billed monthly. 
  	 

### 2.2 Using VM CLI

For all the configuration on our website, we will ssh to root from our own VM. DOcean has an option to access the
the droplets CLI directly through its website. Which raises the following question: 
why not use DOcean CLI instead of accessing root via our own VM? For many reasons actually,

DOcean CLI: 
	
- does not colour syntax; makes coding a lot harder
- major input lag; everything takes twice as long

Regardless, even if we used DOcean CLI we will still need to ssh root from our own machine for things like scp
which we will need later down the line. 

#### 2.2.1 SSH to Root 

Can choose whether you want to SSH root with password or SSH Keygen,
<br>I went with using a root password much eaiser than using the public key everytime.
You can reset the root password at any point through DOcean. 

- Super basic stuff here
- on our own VM
- `sudo ssh root@IPV4AddressOfYourDroplet`
- follow prompts
- your now in your droplets CLI

#### 2.2.2 Apache2 config

- We need some sort of open-source web server software
- went with apache2

`sudo apt install apache2`


### 2.3 Obtaining and Applying a Domain

- Obviously we do not want people to get onto our website via the IPv4 address
- Obtaining a custom domain is quite a smooth and simple process
- decided to use NameCheap for getting custom domain name
- managed to purchase savorapoll.com
- Expires 20/04/2026
 
 
#### 2.3.1 NameCheap + DOcean DNS Host Records

### 2.4 Obtaining TLS/SSL



## *Section 3: Directory Guide*




*useful section for the millions of files*

3.1 Frontend 

3.2 Backend


## *Section 4: Fullstack Overview*



4.3 Architecture

4.4 End-to-End Data flow
	
Frontend: User Clicks Vote Button
	

## *Section 5: Frontend Code* 




5.1 Voting Button (HTML) 
5.2 Submit Vote (JS)
5.3 View Results Button (HTML)
5.4 Load Results (JS)



## *Section 6: Backend Code*


6.1 Route: Submit Vote
6.2 Route: Results
6.3 App Listen
