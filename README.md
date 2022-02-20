# socket-server
Create socket server and use this for live message

Official documentation for socket.io. [click here](https://socket.io/docs/v4/)

<br>
<br>
Folow this instruction step by step, You can be easily create a socket server with node.js.<br>
Lets' start<br>
<b>Step 1</b><hr><br>
First run this command for getting node express file.<br>
```
npm install express@4
```
<br>
then, run this command for getting socket.io file
<br>
```
npm install socket.io
```
<br>
<br>
<b>Step 2</b><hr><br>
Now create a index.js file in your project root folder. Than paste all this code in that file <br>
````<br>
const express = require('express');
const app = express();
const http = require('http');
const server = http.createServer(app);
const { Server } = require("socket.io");
const io = new Server(server);

app.get('/', (req, res) => {
    res.sendFile(__dirname + '/index.html');
});

io.on('connection', (socket) => {
    // socket.broadcast.emit('hi');
    console.log('socket connected');
    socket.on('chat message', (msg) => {
        console.log('message: ' + msg['message']);
        var channel = msg['channel'];
        // socket.broadcast.emit('channel', msg);
        io.emit(channel, msg);
    });
    socket.on('disconnect', () => {
        console.log('user disconnected');
    });
});

server.listen(3000, () => {
    console.log('listening on *:3000');
});
````
<br>
<br>
<b>Step 3</b><hr><br>
Now create a index.html file in your project root folder. Than paste all this code in that file <br>
````<br>
<!DOCTYPE html>
<html>

<head>
    <title>Socket.IO chat</title>
    <style>
        body {
            margin: 0;
            padding-bottom: 3rem;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        }
        
        #form {
            background: rgba(0, 0, 0, 0.15);
            padding: 0.25rem;
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            display: flex;
            height: 3rem;
            box-sizing: border-box;
            backdrop-filter: blur(10px);
        }
        
        #input {
            border: none;
            padding: 0 1rem;
            flex-grow: 1;
            border-radius: 2rem;
            margin: 0.25rem;
        }
        
        #input:focus {
            outline: none;
        }
        
        #form>button {
            background: #333;
            border: none;
            padding: 0 1rem;
            margin: 0.25rem;
            border-radius: 3px;
            outline: none;
            color: #fff;
        }
        
        #messages {
            list-style-type: none;
            margin: 0;
            padding: 0;
        }
        
        #messages>li {
            padding: 0.5rem 1rem;
        }
        
        #messages>li:nth-child(odd) {
            background: #efefef;
        }
    </style>
</head>

<body>
    <ul id="messages"></ul>
    <form id="form" action="">
        <input id="input" autocomplete="off" /><button>Send</button>
    </form>
</body>
<script src="/socket.io/socket.io.js"></script>
<script>
    var socket = io('http://localhost:3000');
    var form = document.getElementById('form');
    var input = document.getElementById('input');

    form.addEventListener('submit', function(e) {
        e.preventDefault();
        if (input.value) {
            data = {
                message: input.value,
                channel: 'message'
            }
            socket.emit('chat message', data);
            input.value = '';
        }

    });

    socket.on('message', function(msg) {
        // alert(msg['channel']);
        // alert(msg['message']);
        var item = document.createElement('li');
        item.textContent = msg['message'];
        messages.appendChild(item);
        window.scrollTo(0, document.body.scrollHeight);
    });

    // socket.on('chat message', (data) => {
    //     console.log("got one hit");
    //     console.log('message: ' + data);
    // });
</script>

</html>
````
<br>
<br>
<b>Step 4</b><hr><br>
We're almost done. Now go to Your command shell than run this command- <br>
 ```
 node index.js
 ```
 <br>
 <br>
 hurray!<br>
 So, you can test now by open your project in browser. <br>
 Or Browse this link<br>
 ```
 http://localhost:3000/
 ```
 <br><br>
 Thanks,<br>Zunaid Miah<br>
