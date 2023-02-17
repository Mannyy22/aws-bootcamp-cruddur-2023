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
![AWS credentials picture](https://user-images.githubusercontent.com/46639580/219569432-2e6b1404-043c-4dc3-87f3-21d1f896c2fb.png)

![gitpod-env](https://user-images.githubusercontent.com/46639580/219686195-09c9f057-0466-4491-a74d-a65e84e2c143.png)

<p>If I type out env | grep AWS my credentials I saved to gitpods variables on my account will be displayed.</p>

## Installed AWS CLI
![InstallCLI](https://user-images.githubusercontent.com/46639580/219565575-72ada5aa-08c0-4980-b93f-072e3b2f5492.png)

<p>Ran the following commands below to install AWS CLI</p>

```bash
  - curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  - unzip awscliv2.zip
  - sudo ./aws/install
```
<p>Ref:https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html </p>

![verifyCLI](https://user-images.githubusercontent.com/46639580/219688520-90785f9d-c65e-4980-a208-714e4a28eaa3.png)
 
 Verifyed my AWS CLI was working by running ```aws sts get-caller-identity```
 
## Create Billing Alarm
![alarm](https://user-images.githubusercontent.com/46639580/219567928-37ee07d8-ae15-463e-8f94-7377e8e6096d.png)


## Create a Budget
![BUDGETS](https://user-images.githubusercontent.com/46639580/219567208-917f7b37-a6f1-429b-93c7-a113d65f1f3e.png)


## Obstacles and Solutions
<ul>
  <li>
   I Was getting You don't have permissions to push to "Mannyy22/aws-bootcamp-cruddur-2023" on GitHub. Would you like to create a fork and push to it             instead? -To fix  I went to Gipod usersettings, intergrations, select Github(three dotted line), edit permissions and selected read:user, public_repo,       repo </li>
  <li>Addin  task to .gitpod.yml file was not launch when booting up gitpod from website, after launching gitpod from github the file sucessfully ran </li>
 </ul>

## Purpose of Cruddar:

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


## Logical Design -

![Cruddar Logical Diagram](https://user-images.githubusercontent.com/46639580/219693878-8d911af8-36de-4605-873d-496fe904c257.png)

