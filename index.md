## Welcome!
This tutorial is designed to help people with minimal programming experience to learn how to program Discord bots with JavaScript. Python tutorials will be added soon as well for those interested in the Python language.

### IDE
The easiest way to get a bot up running is on an online programming environment called [repl.it](https://repl.it/). Create an account, and make a **new repl** with the language NodeJS (JavaScript for the server). An IDE is an integrated development environment, basically where you code and develop your application.

Now that you have your coding environment set up, we need to create the bot on Discord's end before we can program it. Keep this tab aside and navigate to the [Discord Developer Dashboard](https://discordapp.com/developers/applications/).

### Creating a bot account

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

**More lessons will be added soon.**
