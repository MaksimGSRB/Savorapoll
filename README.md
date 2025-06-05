# Savorapoll

## Contents

### Breif Overview
### Documentation 

## Brief Overview of Savorapoll

>Savora is a self-hosted web application, it is designed to allowed users to participate in anonymous opinion polling. Savora emphasises privacy, simplicity and honesty, with regards to modern technology, these values can be difficult to come by. With no unique user information combined with a straightforward simple polling operation, Savora fosters an environment in hopes of users being completely honest with their vote on each poll.<

>Users will be able to enter the site, cast one vote on each of the three polls that are available to users accessing the site. Users will only be able to cast one vote for each of the three different polls. At the end of the week, the poll results are shown with the percentage of users selecting each option for each poll. Users will not be able to input any information or comments regarding any of the polls. Once a user has casted their votes, they will not be able to see what they voted for afterwards and not be able to vote again until the new poll's are released.<

>Savora's primary goal is that users will strive to be completely honest and open with their opinions regardless if a topic is controversial or anodyne. Over time, with users seeing the poll votes on topics, I have hopes that it will encourage real world conversation amongst individuals encouraging honest opinions on a variety of topics with little to no prejudice.<

>In other words, Savora will indirectly establish this philosophy or 'initial condition' through it's website. Over the course of time, fostering this philosophy to individuals in the real world with no-intervention.<


## Preparation

Website hosted on DigitalOcean

## Obtaining and Applying Custom Domain

* Used namecheap for the custom domain
* NameCheap only used for the purchase of the domain
  * thus/i.e. DigitalOcean was used to host the DNS server

Record should should the following DNS record

![image](https://github.com/user-attachments/assets/c533a8fd-3218-4746-91a5-17b5d87d1d16)


## Obtaining SSL

First: Verify following ports are open
`22 = ssh`

`80 = http`

`443 = https`

in CLI

`apt update`

`sudo apt install certbot python3-certbot-apache -y`

`sudo certbot --apache`

autodetected both domains (www.savorapoll.com) (savorapoll.com)

followed prompts 

Certificate should now apply and auto-renew every 90 days.

Verified by manually typing in the full domain (https://www.savorapoll.com) (do so for both)
