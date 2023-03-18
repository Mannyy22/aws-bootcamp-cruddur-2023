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

```
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
I add the following below to my `SignupPage.js`  to intergrate my custom signup page

    ```
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
