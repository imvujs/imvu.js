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

const imvu = new IMVU({ name: 'defaultbot', walk: true, gameslib: false, outfit: [80] });

imvu.on('ready', async () => {
  console.log(`Bot ${imvu.display_name} with ID ${imvu.id} successfully connected to room ${imvu.room.id}`);
  const furn_id = 80;
  const furniture = await imvu.furnitures.spawn(furn_id);
  console.log(`Furniture instance ID: ${furniture.instance_id}`);
});

imvu.login(TOKEN)
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

imvu.on('seat', async (event) => {
  const { furniture } = event.seat;
  const { furniture: furniture_leave } = event.leave;

  if (furniture.instance_id === 35 && furniture.user.isMod) {
    const mod_room = { x: 100, y: 5000 };
    furniture.set.y(mod_room.y).x(mod_room.x);
  } else {
    // Send a whisper
    furniture.user.send('[Bot] - Please leave this spot or you will be kicked in 5 seconds');
    const userId = furniture.user.id;

    setTimeout(async () => {
      if (furniture.user.id === userId) {
        const debug = await furniture.user.kick();
        console.log(debug);
        // {error: 'Bot does not have moderator permissions in your room'}
      }
    }, 5000);
  }

  if (furniture_leave.instance_id === 35) {
    furniture.set.x(0).y(0).z(0).roll(1).pitch(0).yaw(0);
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


