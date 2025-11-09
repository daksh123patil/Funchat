<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FunChat</title>

<style>
  body {
    font-family: Arial, sans-serif;
    background: #0d0d0d;
    color: #fff;
    margin: 0;
    padding: 0;
    text-align: center;
  }

  header {
    padding: 40px 20px;
    background: #111;
    border-bottom: 2px solid #333;
  }

  h1 {
    font-size: 48px;
    color: #00eaff;
    margin-bottom: 10px;
  }

  p {
    max-width: 600px;
    margin: auto;
    font-size: 18px;
    line-height: 1.6;
  }

  nav a {
    color: #00eaff;
    font-weight: bold;
    margin: 0 10px;
    text-decoration: none;
  }
  nav a:hover {
    text-decoration: underline;
  }

  .chat-container {
    width: 360px;
    margin: 40px auto;
    background: #181818;
    padding: 15px;
    border-radius: 12px;
    border: 1px solid #333;
    box-shadow: 0 0 10px rgba(0,255,255,0.3);
  }

  #chat-box {
    height: 350px;
    background: #1e1e1e;
    border-radius: 8px;
    padding: 10px;
    overflow-y: auto;
    text-align: left;
  }

  .message {
    margin: 8px 0;
    padding: 10px;
    border-radius: 6px;
    max-width: 80%;
    font-size: 15px;
  }

  .user {
    background: #007bff;
    color: #fff;
    margin-left: auto;
  }

  .bot {
    background: #333;
    color: #00eaff;
  }

  .input-area {
    display: flex;
    margin-top: 10px;
  }

  input {
    flex: 1;
    padding: 10px;
    background: #222;
    color: #fff;
    border: none;
    outline: none;
    border-radius: 6px 0 0 6px;
  }

  button {
    padding: 10px 20px;
    border: none;
    background: #00eaff;
    color: #000;
    font-weight: bold;
    cursor: pointer;
    border-radius: 0 6px 6px 0;
  }

</style>

</head>
<body>

<header>
  <h1>FunChat</h1>
  <p>FunChat is a free, fast AI chatbot template for web developers and creators.</p>
  <nav>
    <a href="#features">Features</a> ·
    <a href="#deploy">Deploy</a> ·
    <a href="#run">Run Locally</a>
  </nav>
</header>

<div class="chat-container">
  <div id="chat-box"></div>
  <div class="input-area">
    <input type="text" id="user-input" placeholder="Type a message...">
    <button onclick="sendMessage()">Send</button>
  </div>
</div>

<script>
function sendMessage() {
  const input = document.getElementById("user-input");
  const text = input.value.trim();
  if (!text) return;

  addMessage(text, "user");
  input.value = "";

  setTimeout(() => {
    addMessage(botReply(text.toLowerCase()), "bot");
  }, 600);
}

function addMessage(msg, sender) {
  const box = document.getElementById("chat-box");
  const div = document.createElement("div");
  div.className = "message " + sender;
  div.innerText = msg;
  box.appendChild(div);
  box.scrollTop = box.scrollHeight;
}

function botReply(input) {
  if (input.includes("hi") || input.includes("hello"))
    return "Hey! Welcome to FunChat 🤖";
  if (input.includes("name"))
    return "I am FunChat Bot!";
  if (input.includes("bye"))
    return "Goodbye! 👋";
  return "FunChat is still learning! 😄";
}
</script>

</body>
</html>


