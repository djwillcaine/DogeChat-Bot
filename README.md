DogeChat-Bot Module
===================
This module can be used to make a chat bot for the website [DogeChat](http://dogechat.net/) using Node.JS and it simplifies the process a great deal. Please not the following when making a bot for DogeChat:

  * A bot account must have a username ending in bot.
  * Bot accounts must be registered as bots - contact a moderator on DogeChat to arrange this.
  * A bot must not make more than 2 sequential unprompted messages.

Installation
------------

To install the module and its dependencies, create a folder to make you bot in and open your command line there. Then type `npm install dogechat-bot` to install the module.

Getting Started
---------------

You will first need to sign up for a new account, following the rules above, on DogeChat. Next you should create a room to host your bot in, this is usually the same as your bots name.
To start the script make a javascript file containing the following code and run this file to start your bot. Below is an example of a simple bot.

    var bot = require('dogechat-bot');
    bot.connect(BOT_USERNAME, BOT_PASSWORD, function() {
		bot.joinRoom("awesomebot");
		bot.chat("Bot is online!", "awesomebot");
	});
	
	bot.addCommand("!help", function(data) {
		bot.chat(data.user + ": Welcome to my bot! This message is displayed when a user types !help.", data.room);
	});

API
---

### bot.addCommand(command:String, fn:Function(data))
This adds a command to the command registry so that when a user types `command` the function `fn` will be executed.

    bot.addCommand("!command", function(data) {
		// Handle what to do with the command here.
	});

`data` contains an object with some information about the message received.

  * `user` - The user who sent the message
  * `message` - A neatly formatted version of the message received
  * `messageArray` - An array containing the message split up by spaces to make handling parameters easier
  * `room` - The room in which the message was posted
  * `timestamp` - An ISO-8601 formatted datetime string of when the message was sent

### bot.chat(message:String, room:String)
This will add `message` to an internal buffer to output it and chat the message in `room`.

    bot.chat("Hello, world!", "awesomebot");

### bot.tip(user:String, amount:Integer, room:String[, message:String])
This will tip `user` the specified `amount` of dogecoins in the room `room` with the optional message: `message`.

    bot.tip("cainy", 20, "awesomebot", "Thanks for the cool node module!");

### bot.onTip(fn:Function(data))
This specifies a function to be called when the bot receives a tip. The data object contains the following information:

  * `user` - The user who tipped you
  * `amount` - How many dogecoins they tipped you
  * `message` - The message they specified (if any)
  * `room` - The room the user tipped you in
  * `timestamp` - An ISO-8601 formatted datetime string of when the message was sent

    bot.onTip(function(data) {
		// Handle what to do when you receive a tip.
	});

### bot.joinRoom(room:String)
This will send a request for your bot to join `room`.

    bot.joinRoom("awesomebot");

### bot.quitRoom(room:String)
This will quit the room `room`.

    bot.quitRoom("awesomebot");

### bot.getBalance()
This returns the bots balance. This is unfortunately often inaccurate because DogeChat does not always send you every balance update and because a bot is likely to send and receive many tips in quick succession this can become quite inaccurate. However, immediately after logging in it will be accurate.

    var myBalance = bot.getBalance();

### bot.setColor(color:String)
This will set the color your bot messages with. Obviously you must have bought this color on the bots account otherwise the color will default to black.

    bot.setColor("a22");

### bot.setBadge(badge:String)
This will set the badge your bot messages with. Obviously you must have bought this badge on the bots account otherwise the badge will default to none.

### bot.setLogLevel(logLevel:Integer)
This sets how detailed the logging will be made by the module. This will default to `1` if not set.

  * `0` - No logs will be made at all.
  * `1` - Only important information and warnings will be logged
  * `2` - The above as well as debug logs
  * `3` - All of the above as well as any chat messages/tips received