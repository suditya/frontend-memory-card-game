<template>
  <div >
  <form action="" name="login_form" class="login-form" method="post">
    <h1>Login to Your Account</h1>
    <input type="text" placeholder="Username" aria-label="Username" name="username" v-model="username" required autofocus>
    <input type="password" placeholder="Password" aria-label="Password" name="password" v-model="password" required>
    <button type="submit" :disabled="!(username.length && password.length)">Login</button>
  </form>
</div>
</template>

<script>
// import url("https://fonts.googleapis.com/css?family=Open+Sans:400,700");
export default {
    name:'LoginPage1',
    data() {
        return {
        username: "",
        password: ""
        }
    }
}
</script>

<style>
* { box-sizing: border-box; font-family: 'Open Sans', sans-serif; }

body { 
  background-color: #fff;
}

h1 { 
  font-size: 18px; 
  font-weight: normal;
  margin: 0; 
  padding: 0; 
  margin-bottom: 20px;
}

form.login-form {
  background-color: #fff;
  border-radius: 5px;
  margin: 50px auto 0 auto;
  padding: 25px 15px;
  text-align: center;
  width: 100%;
}

input, button {
  border-radius: 3px;
}

input {
  border: 1px solid #dbd9d9;
  display: block;
  font-size: 12px;
  margin-bottom: 15px;
  outline: 0;
  padding: 9px 12px;
  width: 100%;
  
}
input:focus {
    border: 1px solid darken(#dbd9d9, 20%);
    outline: 0;
  }

button {
  background-color: #9bcb3b;
  border: 0;
  font-size: 14px;
  font-weight: bold;
  height: 50px;
  margin-top: 10px;
  outline: 0;
  text-transform: uppercase;
  width: 100%;
  
}
button:hover {
    background-color: darken(#9bcb3b, 10%);
    cursor: pointer;
  }
button:active { 
    background-color: darken(#9bcb3b, 20%);
  }
button[disabled] {
    background-color: darken(#9bcb3b, 20%);
    color: lighten(#000, 10%);
    cursor: auto;
  }
@media (min-width:768px) {
  body { background-color: #dadbdd; }
  form.login-form {
    margin-top: 150px;
    width: 375px;
  }
}
</style>