---
title: Call phone from app
products: stitch
description: "Call a phone from an app. This is the simple version."
languages:
    - Node
---

# Intro

Intro blurb

## In this tutorial

In this tutorial you see how to build a Stitch application that can call a phone:

1. Create a Nexmo Application
2. Buy a number
3. Link a number
4. Create a webhook server
5. Create a Stitch app using Node

## Prerequisites

In order to work through this tutorial you need:

* A [Nexmo account](https://dashboard.nexmo.com/sign-up)
* The [Nexmo CLI](https://github.com/nexmo/nexmo-cli) installed and set up
* A publicly accessible web server so Nexmo can make webhook requests to your app. If you're developing locally you can use [ngrok](https://ngrok.com/).

## 1. Create a Nexmo Application

To create a voice application within the Nexmo platform you execute the following command.

``` bash
$ nexmo app:create "Call phone from App" https://example.com/webhooks/answer https://example.com/webhooks/event --save private.key
Application created: aaaaaaaa-bbbb-cccc-dddd-0123456789ab
```

The parameters to the above command are as follows:

* The name of the app "Call phone from App".
* Answer webhook URL that will be called by Nexmo when you make a call.
* Event webhook URL that Nexmo will call back on with details of events.
* The name of the generated private key to be associated with the application.

The output of this command is the Application UUID (Universally Unique IDentifier).

## 2. Buy a Phone Number

For your application to function you need two or more numbers that are placed in adverts. Buy a number as follows using the Nexmo CLI:

```bash
$ nexmo number:buy --country_code GB --confirm
Number purchased: 447700900000
```

## 3. Link the Phone Number to your Nexmo Application

Associate the newly purchased number with the application you've created. This ensures that your application's webhook endpoints are informed when the number is called or any event takes place relating to the number. You can associate a number with your Nexmo application with the following command:

```bash
$ nexmo link:app 447700900000 5555f9df-05bb-4a99-9427-6e43c83849b8
```

## 4. Create a Nexmo Application

Webhook server code:

``` javascript
'use strict';
const express = require('express');
const bodyParser = require('body-parser');
const app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: true}));

app.post('/webhooks/event', (req, res) => {
  console.log(req.body);
  res.status(200).end();
});

app.get('/webhooks/answer', (req, res) => {
    var to_number = req.query.to;
    var nexmo_number = NEXMO_NUMBER;
    var ncco = [
    {
      action: "talk",
      text: "Forwarding you to Tony"
    },
    {
      "action": "connect",
      "from": nexmo_number,
      "endpoint": [
        {
          "type": "phone",
          "number": to_number
        }
      ]
    }
  ];
  res.json(ncco);
})

const server = app.listen(3000, () => {
  console.log('Express server listening on port %d in %s mode', server.address().port, app.settings.env);
});
```

Explain above code


## 5. Create your Stitch app

Your Stitch application:

``` html
<!DOCTYPE html>
<html lang="en">

<head>
  <script src="./node_modules/nexmo-stitch/dist/conversationClient.js"></script>
</head>

<body>

  <form id="call-phone-form">
    <h1>Call Phone</h1>
    <input type="text" name="phonenumber" value="">
    <input type="submit" value="Call" />
  </form>

  <script>

    const USER_JWT = 'eyJhbGc...II8B6TbFTvfu4btPtzSHYkzg';

    class ChatApp {

      constructor() {
        this.callPhoneForm = document.getElementById('call-phone-form');
        this.setupUserEvents();
        this.createClient();
      }

      createClient() {
        new ConversationClient({ debug: false })
          .login(USER_JWT)
          .then(app => {
            console.log('DEBUG: Logged into app', app);
            this.app = app;

            app.on("member:call", (member, call) => {
              this.call = call;
              console.log("DEBUG: member:call - ", call);
            })

            app.on("call:status:changed", (call) => {
              console.log("DEBUG: call:status:changed - ", call.status);
            })
          })
          .catch(this.errorLogger)
      }

      setupUserEvents() {
        this.callPhoneForm.addEventListener('submit', (event) => {
          event.preventDefault();
          this.app.callPhone(this.callPhoneForm.children.phonenumber.value);
        })
      }

      errorLogger(error) {
        console.log(error);
      }

    }
    new ChatApp();
  </script>
</body>

</html>
```


Explain above code


## Conclusion

Closing blurb
