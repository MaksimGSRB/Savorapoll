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
 
 
#### 2.3.1 NameCheap + DOcean: Nameservers

- Need to have namecheap or DOcean to host DNS records
- Decided to use DOcean for nameservers
- In namecheap:
  	- Custom DNS
  	- For nameserver 1, 2, 3; input the following as corresponds
 	 	- `ns1.digitalocean.com`
  		- `ns2.digitalocean.com`
  		- `ns3.digitalocean.com`

#### 2.3.2 DNS Records

- With the nameserver setup and pointing to DOcean
- Now need to have correct DNS records for our site
- The screenshot below lays out the current settings for the Savorapoll domain


  ![image](https://github.com/user-attachments/assets/0714e631-d386-49c3-afb7-dfa6a96ef132)


Regarding the image above, lets explain each record, line by line

Line 1:
- Type:
	- CNAME (Canonical Name) (record type that points one domain to another)
- Value:
	- www.savorapoll.com points to savorapoll.com
  	- Thus: someonetypes in www.savorapoll.com, directs to savorapoll.com (the domain we purchased)
- TTL (Time To Live) (basically a stop watch for how long to keep a DNS record)
	- in seconds and not lowered to 1800 (default was much higher)
   	- since were currently in a stage of doing frequent changes to our website
   	- Decided to stick with 1800 so the DNS Cache updates frequently (same for all)

Line 2-4
- Type:
  	- NS (nameserver) (specifies the DNS servers for a given domain)
- Value:
  	- directs to ns1.digitalocean.com (ns2... ns3...)
- TTL:
  	- kept default (1800)

Line 5:
- Type:
  	- A (address) (indicates IP add associated with given domain name)
- Value:
  	- directs to our default IPv4 (134.199.168.41)
- TTL:
  	- kept default (3600)
  



### 2.4 Obtaining TLS/SSL

- Ironically: purchasing the SSL cert from namecheap is not cheap
- decided to use the free and well-known "Let's Encrypt"

#### 2.4.1 Ensure correct ports are open

- Need to have port 80 (HTTP) and port 443 (HTTPS) open
	- If not open by now; run:
	- `sudo ufw allow 80/tcp`
	- `sudo ufw allow 443/tcp`
	- `sudo ufw enable`

- For future reference;
- If need to check port 80 and port 443 are open
	- `curl -I http://localhost`
	- `curl -I https://localhost`

- or alternatively (eaiser)

	- `sudo ufw status`
	- should show something along the lines of...
	- `80/tcp allow`
	- `443/tcp allow`

#### 2.4.2 Certbot + Let's Encrypt 

- In CLI
	- `sudo apt update`
	- `sudo apt install certbot python3-certbot-apache -y`
	- `sudo certbot --apache`
- follow prompts so that it will apply to both domains (www.sav... + sav...)

#### 2.4.3 Update Apache2

- Will now need to update the virtual host config
	- `cd /etc/apache2/sites-available/`
	- `nano 000-default.conf`
	- replace to ServerName savorapoll.com
	- replace to ServerAlias www.savorapoll.com

- do the same for:
	- `000-default-le-ssl.conf`
	- `default-ssl.conf`
	- (both located in same dir)

## *Section 3: Directory Guide*

- For simplicity and future reference

  	- whenever referring to **apache2 config**
 	- `/etc/apache2/sites-available`

	- for mostly everything else were focusing on
   	- `/var/www/html`

## *Section 4: Fullstack Overview*

### 4.3 Architecture


![image](https://github.com/user-attachments/assets/c6b3ed8c-1b92-4e7e-b507-adb75c831aef)



### 4.4 End-to-End Data flow

**_Voting Function_**

Frontend: 
	
> User Clicks Vote Button -->

Backend: 
	
> Receives POST /submit-vote -->
>		
>> saves vote in votes.json -->

**_View Results Function_**

Frontend: 
	
> User clicks view results button -->

Backend: 
	
> Receives GET /results -->
>		
>> reads votes.json -->
>>
>>> calculates % -->
>>>
>>>> sends JSON -->

Frontend: 
	
> Displays formatted results to user
  
	

## *Section 5: Frontend Code* 


### 5.1 Voting Button (HTML) 

```HTML

<button class="option-btn" onclick="handleVote('q1', 'Yes')">Yes</button>
<button class="option-btn" onclick="handleVote('q1', 'No')">No</button>
<p id="result-q1"></p>


```


- Basic HTML for a vote button
- note the `handleVote(...`


### 5.2 Submit Vote (JavaScript)

- Pretty standard JS stuff here


```js

function handleVote(questionId, choice) {
	if (votedQuestions[questionId]) return;

votedQuestions[questionId] = true;

document.getElementById(`result-${questionId}`).textContent = `You voted: ${choice}`;
document.querySelectorAll(`.option-btn[onclick*="${questionId}"]`).forEach(btn => btn.disabled = true);

fetch('/submit-vote',{
	method:'POST',
	headers: { 'Content-Type': 'application/json' },
	body: JSON.stringify({ question: questionId, vote:: choice})
		});
	}
```



5.3 View Results Button (HTML)


5.4 Load Results (JavaScript)



## *Section 6: Backend Code*


6.1 Route: Submit Vote
6.2 Route: Results
6.3 App Listen
