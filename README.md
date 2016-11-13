# ChatroomV2

NodeJS api written to teach CUAppDev training program about interacting with APIs from iOS

 *  Install NodeJS from : https://nodejs.org/en/download/
 * Start a terminal window and type the following commands : 
```sh
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
