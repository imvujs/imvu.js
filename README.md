<p align="center">
  <a href="#"><img src="https://imvujs.pages.dev/logo.png" width="546" alt="imvu.js" /></a>
</p>

## About imvu.js

**imvu.js** is a project designed to interact with your own public IMVU rooms and their users. This project is:

- Object-oriented
- Lightweight
- Friendly for new developers

## Installation

To install **imvu.js**, run the following command in your terminal:

```bash
npm i imvu.js
```

## Getting Your Token
To obtain your token, visit our platform [here](https://imvu.js.org) and create it for FREE.

## Login Example
Here is an example of how to log in and spawn furniture:


```javascript
const IMVU = require('imvu.js')
   const imvu = new IMVU({ name: 'MyBot', walk: true });
   imvu.on('ready', () => {
     console.log(`Bot ${imvu.display_name} with ID ${imvu.id} successfully connected to room ${imvu.room.id}`);
	 //Bot IMVUJS with ID 123 successfully connected to room 321
   });
   imvu.login('YOUR_TOKEN');
```

## Usage Examples
Here are some examples of how to use **imvu.js**:


```javascript
imvu.on('message', async (message) => {
  if (message.user.bot) return;

  if (message.content.startsWith('/Ping')) {
    await message.room.send('Pong');
    // Additional action, if necessary...
    await message.user.kick();
  }
});


imvu.on('seat', async (data) => {
  const { seat, leave, user } = data; 
  // An elevator with auth?
  if (seat.instance_id === 35 && user.isMod) {
    const mod_room = { x: 100, y: 5000 };
    seat.set(mod_room)
  } else {
    // Send a whisper
    user.send('[Bot] - Please leave this spot or you will be kicked in 5 seconds');
    setTimeout(async () => {
      if (user.furniture.id === seat.id) {
        const debug = await user.kick();
        console.log(debug);
        // {error: 'Bot does not have moderator permissions in your room'}
      }
    }, 5000);
  }
  // Lets move it back
  if (leave.instance_id === 35) {
    seat.set({
      x:0,
      y:0,
      z:0
    })
  }
});


imvu.on('join', async (user) => {
  if (user.bot) return;

  // Welcome message
  imvu.room.send(`Welcome ${user.display_name} to my wonderful room`);
  user.room.send(`Welcome ${user.display_name} to my wonderful room`);
  // Or... perhaps a whisper?
  user.send(`Welcome ${user.display_name} to my wonderful room`);
});

imvu.on('leave', async (user) => {
  if (user.bot) return;
  // Additional actions, if necessary...
});


imvu.on('furniture_update', async (furniture) => {
  // Additional actions, if necessary...
});
```

For more references and example codes, check the complete documentation.


