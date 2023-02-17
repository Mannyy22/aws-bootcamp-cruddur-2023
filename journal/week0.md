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

<p>If I type out env | grep AWS my credentials I saved to me environment will be displayed.</p>

## Installed AWS CLI

## Create Billing Alarm

## Create a Budget
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

