# ChatroomV2

NodeJS api written to teach CUAppDev training program about interacting with APIs from iOS

 *  Install NodeJS from : https://nodejs.org/en/download/
 * Start a terminal window and type the following commands : 
```sh
$ sudo npm install -g express
$ sudo npm install -g express-generator
```
Note that these two commands may require you to type in your mac password.  As you type your password, it WILL NOT appear on the screen.  BUt fear not, if you type it and hit ENTER, it will work. 

```js
$ cd ~
$ express Chatroom
$ cd Chatroom
```
If you are unfamiliar with terminal, all this does is initialize a new Node project (called Chatroom) in your home directory and then changes your working directory to be inside the project.  Now you can run Node commands! :) 

*  Now, we need to install a few necessary NPM Modules. NPM is the equivalent of Cocoapods -- it manages your external dependencies.  Run the following command : 
```sh
$ npm install firebase-admin --save
$ npm install
```
The `--save` flag saves it in the dependency list in package.json. Now we have a midule that helps us connect to firebase (our Database service) At this point, you should open up the project in a text editor (like sublime or atom)

* We are going to switch gears for a second and actually set-up a database to connect to.  Leave your terminal window open but open https://console.firebase.google.com/?pli=1 in your browser. 
* Click "Create new Project" and select a Name for your project.   Click "Create New project" again. 

* Click on the gear on the top left and select "Project Settings"
* From the top menu, select "service accounts".  Then click 'Generate new private key"  Save the file that is downloaded as "database_key.json" inside your Chatroom folder
* Now, copy that code that popped up in the firebase window
* Go back Chatroom, which you opened in the text editor. Open the inde.js file and copy and paste the code right after the first two lines fo your index.js file. It should look like this now (but with a different db url) : 
```js
var express = require('express');
var router = express.Router();

var admin = require("firebase-admin");
admin.initializeApp({
  credential: admin.credential.cert("path/to/serviceAccountKey.json"),
  databaseURL: "https://chatroom-96977.firebaseio.com"
});
```
* Now, change the `path/to/serviceAccountKey.json`  to `./database_key.json`

* Finally, add one more line of code that tells the server we are using the Database part of Firebase.  Right under the section of code you just added, add 
```js
var db = admin.database();
```
* Let's test the database connection! In this step, you are also going to learn how to use PostMan, a tool for sending test HTTP requests.  In the index.js file, write the following code : 

```js
router.post('/messages', function(req, res, next) {
  db.ref().child('/messages').push({
    sender : req.body.sender,
    created_at : Date.now(),
    content : req.body.content
  }, function () {
    res.status(200).json({message : "success"});
  });
});
```
* Save the file and. From your terminal window again, run ` $npm start` you should see this in your terminal: 
```
> Chatroom@0.0.0 start /Users/administrator/CUAppDev/ChatroomV2
> node ./bin/www
```

* Now, open PostMan.  It is a google chrome extension. You can add it to chrome here : https://www.getpostman.com/
I have never used the mac app before, but if you really don't like chrome, you can try it. 

* Here is a collection of test requests for this project : https://www.getpostman.com/collections/b45a697d1c0a99433b51

* Go into the "Post Messages" tabl on the left side. In the "body" section, edit the body to be whatever you want. It should still include a sender and a message.  

* Click "Send."  You should see a "success" message pop up in Postman.  It may take a second or two. Then, if you go to your Firebase Database console, you should see the messageyou just posted! 

* Now, go back to the index.js file.  Now that we can write data to the database, you want to be able to Read data.  Add the following code to index.js 

```js
router.get('/messages/', function (req, res, next){
  db.ref('/messages').once('value').then(function(data) {
    res.status(200).json({messages : data.val()})
  });
});
```
* Restart the server by typing Ctrl^C in the terminal and running ` $ npm start` again.  You have to do this every time you change any code. 
* Now, you should be able to run the test GET request from postman.  

* NOW, we can go one stop further : open up this collection of postman tests. Instead of All of these requests sending to your guys' Firebase projects, they are all directed at my server.  You can actually simulate a chatroom this way! 

* Obviously, we don't want to ONLY interact with our server via Postman.  However, next week you are going to learn how to connect to 3rd party libraries from the iOS side of things. 

* But even this really simple app has a lot of potential for added features -- we could add "groups" that users belong to (similar to groupme) pretty easily or add login functionality to keep the system secure.  We can do all of this JUST with firebase and a few more sections of data. 

* But it doesnt stop there -- Firebase has some pretty crazy realtime functionality built in. If you want to explore them, there are a ton of great tutorials on how to get started. 

