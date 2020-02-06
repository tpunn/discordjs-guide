## Welcome!
This tutorial is designed to help people with minimal programming experience to learn how to program Discord bots with JavaScript. Python tutorials will be added soon as well for those interested in the Python language.

### IDE
The easiest way to get a bot up running is on an online programming environment called [repl.it](https://repl.it/). 

Create an account, and make a **new repl** with the language NodeJS (JavaScript for the server). An IDE is an integrated development environment, basically where you code and develop your application.

*Why repl.it?* To make a functioning Discord bot, you need to pay for 24/7 hosting services. Repl.it, along with other third-party services, allows us to easily and reliably host our bot (and provide an IDE to work on it) for free. A major downside though is not being able to work with voice channels, therefore you can't make a music bot at least in my knowledge. There might be a hacky way to get this done, but the right way is not supported unfortunately. Otherwise, repl.it is ideal for getting your first bots and command-dominant bots online.

### Creating a bot account

Now that you have your coding environment set up, we need to create the bot on Discord's end before we can program it. Keep this tab aside and navigate to the [Discord Developer Dashboard](https://discordapp.com/developers/applications/).

1. Create a "new application" with the large blue button at the top right near your avatar.
2. Fill in the name field and create your app. On the left menu, navigate to "Bot"
3. Click on the Add Bot button. It will warn you that this is an irreversible action. Proceed.
4. Change your bot settings, like the name and avatar that will show up on their account. Copy the client secret and save it somewhere. This token shouldn't be publicized and secured, or people can use your bot to harm the server.
5. Go to the OAuth2 tab, and scroll down to the scopes section of the URL generator. Select the "bot" checkbox and copy the URL.
6. Navigate to the URL, and select the server you want to add the bot to. You must be an admin of the server. It will ask for a Recaptcha form before adding the bot. Complete this, and wait till the "Authorized" message comes up. You have added the bot to a server! Check the server and see if a new member joined with your bot identity.

### Basic Programming

You probably noticed your bot is offline. Your code will program the bot to log in itself, and give it actions based off events. The main one we will use is message, as commands sent by other users is how a bot reacts to user input.

First, add a new file called `.env`. A dotenv file can contain any information you want that repl.it will hide from the public to see. This is ideal for tokens and passwords that are secret, such as, ahem, the client secret you got earlier. The format is fairly basic:
```
ITEM=value
ANOTHER_ITEM=value2
```
Except you would put
```
TOKEN=Whatever your client secret is
```
Accessing dotenv values in Node.js is easy, and you just need to use `process.env.NAMEOFVALUE` or in this case... `process.env.TOKEN`

In your `index.js` file, add the following starter code:
```js
const Discord = require('discord.js') //Imports the discord.js library so you can use special features from Discord
const client = new Discord.Client() //creates a new client for your program to run on
const prefix = "!" //Defines the command prefix... `!hello` for example

const.once('ready', () => console.log('Ready!')) //This prints a message in the console (bottom right of IDE) once the program is ready

//Whenever you send a message, it will create a function sending the `message` object in its scope.
const.on('message', message => {
  if (!message.content.startsWith(prefix) || message.author.bot) return; //Returns ends the function. This will happen if the read message does not start with the prefix defined earlier or if the author of the message is a bot
  
  //Commands are formatted so: !commandName argument1 argument2 argument3
  const args = message.content.slice(prefix.length).split(/ +/); //Removes the prefix with slice which removes a set number of characters at the start of a string, and then splits them into an array by spaces.
	const command = args.shift().toLowerCase(); //shift() removes the first item of an array, but will store the removed item into variables. This is efficient because it removes it from the previous args variable, and provides the command separately. Making the command lowercase will allow people to type it in any case without being rejected.
  
  if (command === 'hello') message.reply('Hello!') //Replies to the message, pinging the author
  if (command === 'copy') message.channel.send(args.join(' ')) //Joins all the argument array values together with a space separating them. message.channel.send doesn't ping the author, just sends a message to whichever channel they are in
  if (command === 'add') message.channel.send(parseInt(args[0])+parseInt(args[1])) //array[index] gets the an item from an array (args in this case) by its number of order. note that the 1st item is 0, not 1, and the second is 1, not 2 and so on. parseInt is required because your message is a string, not an integer so you can't add strings (they will concatenate/combine).
})

client.login(process.env.TOKEN)
```

This example goes over the essential components of a basic bot. In future examples, you will see more advanced examples and systems to make your bot cleaner and more complex.

### Keeping your bot alive

You might've noticed that the moment you signed off repl.it, the bot went offline. We wouldn't have chosen repl.it if it can't keep bots online 24/7. Here are some things you should know about what we will be doing:
- Repl.it code can stay alive if a server is active
- Servers go inactive after an hour, so pinging it is required

To do this, we need to use two tools: a server framework (respective to the language, Express-Node.js, Flask-Python) and UptimeRobot. We will go over how to use these tools in this section.

First, to create an active server, make a new file in your repl project named `server.js`

```js
const express = require('express');
const server = express();

server.all('/', (req, res) => res.send('Your bot is alive!'))

const keepAlive = () => {
    server.listen(3000, () => console.log("Server is Ready!"));
}

module.exports = keepAlive;
```
The above code will build a server with Express that can be pinged by UptimeRobot. Since this is in a separate file, and the run will only work with `index.js`, you need to require this file as a module, and get the `module.exports` as you can see on the bottom of the code to execute the function. Add the following lines to your `index.js`

```js
const keepAlive = require('./server');
keepAlive();
```

Now, when you run the project, you should see a new tab pop up left to the code with a link in the address bar. Copy this link, and keep it somewhere to be used later on.

Next, redirect yourself in a new tab to the [UptimeRobot Website](https://uptimerobot.com/).
Create a new account, and once you find success, navigate to the dashboard.

1. Add new monitor
2. In the monitor type field, select "HTTP(s)"
3. Give a friendly name regarding your bot
4. In the URL/IP field, put in the link you copied earlier
5. Since repl.it goes inactive every hour, 30 minutes would be an ideal interval
6. If you want, select the checkbox for alerts which sends your email downtimes and such. 
7. Once you did all the steps above, press on the blue "create monitor" button. If you didn't select an alert field, it will ask you once more with an orange button. Click anyways.

Congratulations, your monitor is now ready! It will ping your bot's server every 30 minutes, keeping it online 24/7 except for a few short downtimes throughout. You officially have a working bot. Future lessons will be focused on refactoring bot code into an easier file based system that runs on modularization/enmapping, as well as adding more complex features into the mix like databases (through Firebase).

**Having trouble getting the bot to work?** Compare your code to mines: https://repl.it/@tpunn19/ExampleDjsBot. If you have any questions, don't hesitate to comment in my Github Repository for this tutorial (link to repo at the top of page).
