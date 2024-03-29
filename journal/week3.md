# Week 3 — Decentralized Authentication

## Todo list
- [x] Watch weekly videos
- [x] Setup Cognito User Pool
- [x] Implement Custom Signin Page
- [x] Implement Custom Signup Page
- [x] Implement Custom Confirmation Page
- [x] Implement Custom Recovery Page
- [x] Verify JWT token server side

## Setup Cognito User Pool
Setup Cognito User Pool using AWS Console
![image](https://user-images.githubusercontent.com/46639580/226080024-06aa9e69-4d22-4e7a-bfff-43dc844662be.png)

## Implement Custom Signin Page
Implemented Congito to our Frontend, by adding code to our App.js file.

I Configureed Amplify, I added the following below to  my `App.js`,

```js
import { Amplify } from 'aws-amplify';

Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    // We are not using an Identity Pool
    // identityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID, // REQUIRED - Amazon Cognito Identity Pool ID
    region: process.env.REACT_APP_AWS_PROJECT_REGION,           // REQUIRED - Amazon Cognito Region
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,         // OPTIONAL - Amazon Cognito User Pool ID
    userPoolWebClientId: process.env.REACT_APP_CLIENT_ID,   // OPTIONAL - Amazon Cognito Web Client ID (26-char alphanumeric string)
  }
});
```

After adding the following to be able to Conditionally show components based on logged in or logged out

Added the following below to your `HomeFeedPage.js`

```js
// [TODO] Authenication
import Cookies from 'js-cookie'

export default function HomeFeedPage() {
  const [activities, setActivities] = React.useState([]);
  const [popped, setPopped] = React.useState(false);
  const [poppedReply, setPoppedReply] = React.useState(false);
  const [replyActivity, setReplyActivity] = React.useState({});
  const [user, setUser] = React.useState(null);
  const dataFetchedRef = React.useRef(false);
  
  const loadData = async () => {
    try {
      const backend_url = `${process.env.REACT_APP_BACKEND_URL}/api/activities/home`
      const res = await fetch(backend_url, {
        method: "GET"
      });
      let resJson = await res.json();
      if (res.status === 200) {
        setActivities(resJson)
      } else {
        console.log(res)
      }
    } catch (err) {
      console.log(err);
    }
  };
```
![image](https://user-images.githubusercontent.com/46639580/226080660-5b5f6892-8ea7-48fa-82d9-3efd46dcda36.png)

## Implement Custom Signup Page
I add the following below to my `SignupPage.js` to intergrate my custom signup page

  ```js
    const onsubmit = async (event) => {
    event.preventDefault();
    setErrors('')
    try {
        const { user } = await Auth.signUp({
          username: email,
          password: password,
          attributes: {
            name: name,
            email: email,
            preferred_username: username,
          },
          autoSignIn: { // optional - enables auto sign in after user is confirmed
              enabled: true,
          }
        });
        console.log(user);
        window.location.href = `/confirm?email=${email}`
    } catch (error) {
        console.log(error);
        setErrors(error.message)
    }
    return false
  }
  ```
  Signing up through Cruddar
  
  ![image](https://user-images.githubusercontent.com/46639580/226086300-a65f6cc3-f4b1-4190-befa-1aed213060b5.png)
  
  I was able to log into new account with Name and Handler to I inputted while signing up
  ![image](https://user-images.githubusercontent.com/46639580/226086453-8b064684-7f57-41e0-ac58-4f198b7f425f.png)

  ## Implement Custom Confirmation Page
  We also added confirmation by adding the following code below to `ConfirmationPage.js`. This would allow us to confirm 
  ```js
   const resend_code = async (event) => {
    setErrors('')
    try {
      await Auth.resendSignUp(email);
      console.log('code resent successfully');
      setCodeSent(true)
    } catch (err) {
      // does not return a code
      // does cognito always return english
      // for this to be an okay match?
      console.log(err)
      if (err.message == 'Username cannot be empty'){
        setErrors("You need to provide an email in order to send Resend Activiation Code")   
      } else if (err.message == "Username/client id combination not found."){
        setErrors("Email is invalid or cannot be found.")   
      }
    }
  }

  const onsubmit = async (event) => {
    event.preventDefault();
    setErrors('')
    try {
      await Auth.confirmSignUp(email, code);
      window.location.href = "/"
    } catch (error) {
      setErrors(error.message)
    }
    return false
  }
  ```
  
  Confirmation that account is in Cognito with account also verfied
  ![image](https://user-images.githubusercontent.com/46639580/226086347-803d9161-f586-4450-a901-3be05054e233.png)
  
  Got email-verfication to verify my account:
  ![image](https://user-images.githubusercontent.com/46639580/226086471-c58168e4-6909-42e1-b880-7b715e18ef1c.png)
  
  ## Implement Custom Recovery Page
 I add the following to `my RecoveryPage.js`. So I could recover my password in case I forgot it.
 ```js
   const onsubmit_send_code = async (event) => {
    event.preventDefault();
    setErrors('')
    Auth.forgotPassword(username)
    .then((data) => setFormState('confirm_code') )
    .catch((err) => setErrors(err.message) );
    return false
  }
  const onsubmit_confirm_code = async (event) => {
    event.preventDefault();
    setErrors('')
    if (password == passwordAgain){
      Auth.forgotPasswordSubmit(username, code, password)
      .then((data) => setFormState('success'))
      .catch((err) => setErrors(err.message) );
    } else {
      setErrors('Passwords do not match')
    }
    return false
  }
  ```
![image](https://user-images.githubusercontent.com/46639580/226086644-51b2477c-7ea4-4337-ae12-5a3c3a743a05.png)


![image](https://user-images.githubusercontent.com/46639580/226086726-090f4665-e25e-4cae-90b5-318797dcef41.png)

Sucessfully reseted my password

![image](https://user-images.githubusercontent.com/46639580/226086906-41e2516f-08f4-4e4e-a701-a3dc4214699e.png)

## Verify JWT token server side
Unauthenticated
![image](https://user-images.githubusercontent.com/46639580/226203350-90c59f66-6bf2-468c-87c6-e5bb79f6f019.png)


Authenticated
![image](https://user-images.githubusercontent.com/46639580/226203103-6f00cb81-f9a9-4991-91b3-b4ea511b2312.png)


![image](https://user-images.githubusercontent.com/46639580/226203278-2ae2ae0b-3f35-4efb-a424-7c9f66280f9b.png)

