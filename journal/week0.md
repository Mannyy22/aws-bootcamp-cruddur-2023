# Week 0 — Billing and Architecture

## Todo list
- [x] Watch weekly videos
- [x] Create Admin User
- [x] Generate AWS Credentials
- [x] Installed AWS CLI
- [x] Create Billing Alarm
- [x] Create a Budget

## Create Admin User
![2023_02_17_00_50_04_IAM_Management_Console_Mozilla_Firefox](https://user-images.githubusercontent.com/46639580/219560841-a71421c2-9060-4c39-89cd-43cbc1ddbbca.png)

<p>I created my admin user via the console, and gave permssion AdministratorAccess to user.</p>

## Generate AWS Credentials
![2023_02_17_00_50_04_IAM_Management_Console_Mozilla_Firefox](https://user-images.githubusercontent.com/46639580)
![AWS credentials picture](https://user-images.githubusercontent.com/46639580/219565091-332f0866-abe9-4e0e-bf05-368787341c91.png)

<p>If I type out env | grep AWS my credentials I saved to gitpods variables on my account will be displayed.</p>

## Installed AWS CLI
![InstallCLI](https://user-images.githubusercontent.com/46639580/219565575-72ada5aa-08c0-4980-b93f-072e3b2f5492.png)

<p>Ran the following commands below to install AWS CLI</p>
<p>curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"</p>
<p>unzip awscliv2.zip</p>
<p>sudo ./aws/install</p>
<p>Ref:https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html </p>

 
## Create Billing Alarm
![alarm](https://user-images.githubusercontent.com/46639580/219567928-37ee07d8-ae15-463e-8f94-7377e8e6096d.png)


## Create a Budget
![BUDGETS](https://user-images.githubusercontent.com/46639580/219567208-917f7b37-a6f1-429b-93c7-a113d65f1f3e.png)


Purpose of Cruddar:

Cruddar is Twitter like platofrm that allows users to set a expirary time on post, this allows people to more freely speak their minds at a current momment.

## Conceptual Design - https://lucid.app/documents/view/78b44e64-df57-4356-9be4-c556c0accaf9

![Cruddar Conceptual Design](https://user-images.githubusercontent.com/46639580/219547089-cf6fcd7c-c54f-4335-ba5d-d8a495cfa8f8.jpeg)

 Throught process:
1) User connects to Internet and access cruddar
2) User can access frontend and search using search engine
3) Front and and backend communicate using API
4) Search engine communicates to Backend
5) Backend store users into Database
6) Messageing System will recieve user info from Database and process stuff to back to personailze user feed. 

