---
layout: page
title: "JavaScript SDK"
---

# Getting Started

If you have already install Node JS, you can directly skip to step 2.

## Step 1 : Install Node JS

Follow the step in the followed link : https://nodejs.org/en/download/

## Step 2 : Init and install the SDK

**Init package.json file**

In your project folder, you have to create a package.json file for add all the dependencies that we need.
Open a Command line and go to your project root and type the folowing command :

- `npm init`

Validate the project features with the enter button and finish to create you package.json file by typing 'yes'.

**Install the javascript SDK**

- `npm install dcd-sdk-js`

## Step 3 Connecting a Thing to the Hub

**Import the javascript SDK tools**

At this stage you need the credentials of the Thing you want to connect to the hub. If you do not have one yet, please sign in/sign up to the DCD Hub and create a Thing following the instructions here.

Right click at the root of your project (left panel) and create a file ‘random-data.js’.

In this file, add the following line to import Javascript SDK tools.

```js
const dcd = require('dcd-sdk-js')
```

**Set the credential** 

Then, we set the credential of our Thing. In Javascript, it means we look at the environment variables to read the id and access token of our thing. To provide these information as environment variable, right click at the root of your project (left panel) and create a file ‘.env’.

In this file, type in the following and paste your id and access token after the equal signs.

```
THING_ID=
THING_TOKEN=
```

Note: If your are using Git, you do not want to track the file ‘.env’ with Git as it contains secrets. To avoid any mistake, the file .gitignore list all files, folders and extensions to ignore. Create a file ‘.gitignore’ and add a new line with ‘.env’.

**Import our credential**

For that we need to import additional node.js packages. It is necessary for the search and reading of our .env file.
In a command line type the folowing command :

- `npm install dotenv`
- `npm install find-config`

Back into our node.js file, we can now import our credential. We load environment variables and access our id and token as follows:


```js
const dotenv = require('dotenv')
const findconfig = require('find-config')

// The thing ID and access token
dotenv.config({ path: findconfig('.env') })
const THING_ID = process.env.THING_ID;
const THING_TOKEN = process.env.THING_TOKEN;
```

Note: In Javascript, any line starting with a ‘/’ is a comment, to help understand what the code does but ignored by Javascript when running the programme.

Next, we can instantiate a Thing with the credentials, with a JSON Object. We store this object in a variable called ‘my_thing’, which we will use to manage our Thing on the DCD Hub.

```js
//Instantiate a thing with its credential
var my_thing = new dcd.Thing({
    id : THING_ID,
    token : THING_TOKEN,
})
```

The following line ‘read’ the details of our Thing, meaning it connects the DCD Hub and asks for the information related to this Thing.

```js
//We can fetch the details of our thing
my_thing.read()
```

We can use the method json() to get a JSON view of the Thing. We show the result in the console with the javascript function console.log(). If you just registered your Thing on the DCD Hub, it has only an Id, a name and a type. Notes that Javascript are synchronous so we have to make a timeout or a promise to show data.

```js
//1) With a time out
    my_thing.read()
    setTimeout(function(){ 
    console.log(my_thing.json())
    }, 2000) //2 seconds before log the result.
```
```js
//2) Or With a promise
    // 1st method with a .then()
    my_thing.read().then(() => {
    console.log(my_thing.json())
    })
```
```js
//2) Or With a promise
    //2nd method with async/await
    async function read_thing(){
    await my_thing.read()
    console.log(my_thing.json())
    }
    read_thing()
```

To create a Property for our Thing, we can use the method find_or_create_property(). This method takes a property name and a property type as parameters, search for a property of the same name in the Thing, and return the property. If no property is found, it requests the creation of a new one on the DCD Hub and returns it. In the following example, we create a property with the name ‘My Random Property’ of type ‘THREE_DIMENSIONS’, meaning that every data point will be compose of three values.

```js
//If we have no properties, let's create a random one
const my_property = my_thing.find_or_create_property("My Random Property",
dcd.PropertyType.THREE_DIMENSIONS)
```

Similar to the Thing, we can display the details of a Property with the method json().

```js
async function create_property() {
    const my_property = await my_thing.find_or_create_property("My Random Property",
dcd.PropertyType.THREE_DIMENSIONS)
    console.log(my_property.json())
}
create_property()
```

Here we also need to resort to asynchronous techniques with find_or_create_property() function. So here is how we can arrange the code from the begining :

```js
my_thing.read().then(async () => {
    console.log('my_thing',my_thing.json())
    const my_property = await my_thing.find_or_create_property("My Random Property",
dcd.PropertyType.THREE_DIMENSIONS)
    console.log('my_property',my_property.json())
})
```

## Step 4 Execute the Javascript code

Let's execute this code. Go to the terminal and type in the following command :

```bash
node random-data.js
```

## Step 5 Sending Data

With this code we are ready to send data to the DCD Hub. To send random data, we are going to use two javascript functions, the first, *Math.random()* for generating random numbers, the second, *setInterval()* to set an interval between scripts (pausing the programme).

We create a function that send random values to the Hub. In this function we create an array with three random values and we call the method update_values(). This method prepare and send the array of data to the Hub.

```js 
//Let's create a function that generate random values
function generate_dum_property_values(the_property){
    //Define an array with 3 random values in range
    const values = [getRandomInRange(0,5,3),getRandomInRange(0,5,3),getRandomInRange(0,5,3)]
    //Update the values of the property
    the_property.update_values(values)
}

//Small function that return a random value between a range and with a fixed precision
function getRandomInRange(from, to, fixed) {
    return (Math.random() * (to - from) + from).toFixed(fixed) * 1;
    // .toFixed() returns string, so ' * 1' is a trick to convert to number
}
```
Finally, we can call this methods infinitely with a interval, waiting 2 seconds after each update.

```js
//Finally, we call our function to start generating dum values
setInterval(function(){
    generate_dum_property_values(my_property)
    },2000) // 2 seconds break
```

You can execute the Javascript script again and check incoming data with
[DCD data subject](/docs/2019/07/31/tool-data-subject).

Back in the terminal, stop your Javascript script with CMD+C (Ctrl+C).

# Services functions

Here are all the functions you can use to manage the Persons, things and property of your application

## Person Service
 ```js
 const dcd = require('dcd-sdk-js')
 const token = 'your_token'

//Read user credentials
dcd.PersonService.readUser(token)
.then((data)=> {
console.log(data)
})

//Read user Id (person_sub)
dcd.PersonService.readUserId(token)
.then((data)=> {
console.log(data)
const person_sub = data['sub']
})

//Revoke login
const person_sub = 'your_person_sub'
dcd.PersonService.logout(person_sub,token)
.then((data)=> {
console.log(data)
})
 ```
## Thing Service

```js
 const dcd = require('dcd-sdk-js')
 const token = 'your_token'

//list all things
dcd.ThingService.list(token)
.then((data)=> {
console.log(data)
})

//Read a things by id
const thing_id = 'your_thing_id'
dcd.ThingService.read(thing_id,token)
.then((data)=> {
console.log(data)
})

//Delete thing with his id
const thing_id = 'your_thing_id'
dcd.ThingService.delete(thing_id,token)
.then((data)=> {
console.log(data)
})

//Create a thing with a thing in json.
//You can set if you want a jwt in response
//Your have to create a Thing with a name
const thing = new dcd.Thing({
    name : 'your_thing_name',
    type : 'your_thing_type'
    description : 'your_thing_description',
    //...
})
const jwt = true
dcd.ThingService.create(thing.json(),jwt,token)
.then((data)=> {
console.log(data)
})
```

## Property Service

```js
//Read a property with his id
const thing_id = 'your_thing_id'
const property_id = 'your_property_id'
const from = (new Date('December 17, 2008 03:24:00')).getTime()
const to = (new Date()).getTime()
dcd.PropertyService.read(thing_id,property_id,from,to,token)
.then((data)=> {
console.log(data)
})

//Delete property with his id
const thing_id = 'your_thing_id'
const property_id = 'your_property_id'
dcd.PropertyService.delete(thing_id,property_id,token)
.then((data)=> {
console.log(data)
})

//Create a property with a thing_id and a property in json.
//You have to create a property with a name and a type of PropertyType
const thing_id = 'your_thing_id'
const property = new dcd.Property({
    name : 'your_property_name',
    type : dcd.PropertyType.LOCATION
    description : 'your_property_description',
    //...
})
dcd.PropertyService.create(thing_id,property.json(),token)
.then((data)=> {
console.log(data)
})

//Update a property with a thing_id, property_id and an an array of values
const thing_id = 'your_thing_id'
const property_id = 'your_property_id'
const ts_now = (new Date()).getTime()
const values = [ts_now,52.0186,4.3782] //example of location values
dcd.PropertyService.update(thing_id,property_id,values,token)
.then((data)=> {
console.log(data)
})
```


# Oauth2 passport Strategy

To create an application that will connect to the hub from Oauth 2 we will use the strategy function :

```js
const passport = require('passport')

const strategyOptions = {
    authorizationURL: process.env.OAUTH2_AUTH_URL,
    tokenURL: process.env.OAUTH2_TOKEN_URL,
    clientID: process.env.OAUTH2_CLIENT_ID,
    clientSecret: process.env.OAUTH2_CLIENT_SECRET,
    callbackURL: process.env.OAUTH2_REDIRECT_URL,
    userProfileURL: process.env.OAUTH2_PROFILE,
    state: true,
    scope: ['offline', 'openid', 'profile', 'dcd:things', 'dcd:persons']
  };

  passport.use('oauth2', new dcd.Strategy(strategyOptions,
    (accessToken, refreshToken, profile, cb) => cb(null, {accessToken, profile})
  ));
```

Example code with an express server :

```js
const dcd = require('dcd-sdk-js')
const express = require('express');
const cookieParser = require('cookie-parser')
const bodyParser = require('body-parser')
const session = require('express-session')
const refresh = require('passport-oauth2-refresh')
const passport = require('passport')
const dotenv = require('dotenv')
const findconfig = require('find-config')

dotenv.config({ path: findconfig('.env') })

// Express server
const app = express();
const PORT = process.env.PORT || 8080;

//Oauth2
const baseUrl = process.env.BASE_URL || '';

const strategyOptions = {
    authorizationURL: process.env.OAUTH2_AUTH_URL,
    tokenURL: process.env.OAUTH2_TOKEN_URL,
    clientID: process.env.OAUTH2_CLIENT_ID,
    clientSecret: process.env.OAUTH2_CLIENT_SECRET,
    callbackURL: process.env.OAUTH2_REDIRECT_URL,
    userProfileURL: process.env.OAUTH2_PROFILE,
    state: true,
    scope: ['offline', 'openid', 'profile', 'dcd:things', 'dcd:persons']
  };

  passport.use('oauth2', new dcd.Strategy(strategyOptions,
    (accessToken, refreshToken, profile, cb) => cb(null, {accessToken, profile})
  ));
  passport.use('refresh', refresh);
  
  passport.serializeUser((user, done) => {
    done(null, JSON.stringify(user))
  });
  
  passport.deserializeUser((user, done) => {
    done(null, JSON.parse(user))
  });
  
  // view engine setup
  app.use(bodyParser.json());
  app.use(bodyParser.urlencoded({extended: false}));
  app.use(cookieParser());
  
  // These are middlewares required by passport js
  app.use(session({
      secret: 'keyboard cat',
      resave: false,
      saveUninitialized: true,
      cookie: { secure: false }
  }));
  app.use(passport.initialize());
  app.use(passport.session());
  
  
  // This is a middleware that checks if the user is authenticated. It also remembers the URL so it can be used to
  // redirect to it after the user authenticated.
  const checkAuthentication = (req, res, next) => {
      // The `isAuthenticated` is available because of Passport.js
      if (!req.isAuthenticated()) {
          req.session.redirectTo = req.url;
        res.redirect(baseUrl+'/auth');
          return
      }
      next()
    };

app.get(baseUrl+'/',checkAuthentication,
async (req, res, next) => {
  //console.log(req)
    console.log('baseUrl')
    res.render('index', { req });
});

app.get(baseUrl+'/auth', passport.authenticate('oauth2'));

app.get(baseUrl+'/auth/callback',

passport.authenticate('oauth2',
{failureRedirect: '/auth'}),
(req, res) => {
// After success, redirect to the page we came from originally
console.log('/auth/callback ' + req.session.redirectTo);
res.redirect(req.session.redirectTo)
}
);

// Start up the Node server
app.listen(PORT, () => {
  console.log(`Node Express server listening on http://localhost:${PORT}`);
});
```
