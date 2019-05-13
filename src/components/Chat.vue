<template>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>Login</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    
    <!-- Custom styles for this template -->
  </head>
  <body class="text-center">
    <form class="form-signin" @submit.prevent="sendMessage">
      <h1 class="h3 mb-3 font-weight-normal">Send A Message</h1>
      <input
        v-model="user"
        id="name"
        class="form-control"
        placeholder="Name"
        required
        autofocus
      >
      <input
        v-model="message"
        id="message"
        class="form-control"
        placeholder="Message"
        required
      >
      <button class="btn btn-lg btn-primary btn-block" type="submit">Send</button>
    </form>
              <div class="card-body">
              <div class="messages" v-for="(msg, index) in messages" :key="index">
                  <p><span class="font-weight-bold">{{ msg.user }}: </span>{{ msg.message }}</p>
              </div>
          </div>
  </body>
</html>
</template>

<script>

import io from "socket.io-client";
export default {
  data() {
    return {
      user: "",
      message: "",
      messages: [],
      socket: io("localhost:3001")
    };
  },
  methods: {
    sendMessage(e) {
      e.preventDefault();

      this.socket.emit("LOGIN", {
        user: this.user,
        message: this.message
      });
      this.message = "";
    }
  },
  mounted() {
    this.socket.on("MESSAGE", data => {
      this.messages = [...this.messages, data];
    });
  }
};
</script>

<style>
html,
body {
  height: 100%;
}

body {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding-top: 40px;
  padding-bottom: 40px;
  background-color: #f5f5f5;
}

.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}
.form-signin .checkbox {
  font-weight: 400;
}
.form-signin .form-control {
  position: relative;
  box-sizing: border-box;
  height: auto;
  padding: 10px;
  font-size: 16px;
}
.form-signin .form-control:focus {
  z-index: 2;
}
.form-signin input[type="email"] {
  margin-bottom: -1px;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
  margin-bottom: 10px;
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
</style>
