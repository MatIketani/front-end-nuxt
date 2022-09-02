<template>
  <body>
    <b-container class="container shadow p-3 mb-5 bg-white rounded">
      <div>
        <b-form @submit="handleLogin">
          <legend>Login</legend>
          <b-form-group
            id="username-input-group"
            label="Username"
            label-for="uname-input"
          >
            <b-form-input
              id="uname-input"
              v-model="form.username"
              type="text"
              placeholder="Username"
              required
            ></b-form-input>
          </b-form-group>
          <b-form-group
            id="password-input-group"
            label="Password"
            label-for="pwd-input"
          >
            <b-form-input
              id="pwd-input"
              v-model="form.password"
              type="password"
              placeholder="Password"
              required
            ></b-form-input>
          </b-form-group>
          <b-form-group id="buttons-group">
            <b-button type="submit" variant="primary">Sign In</b-button>
          </b-form-group>
        </b-form>
        <p>Don't have an account? <a href="/register">Create one here.</a></p>
      </div>
    </b-container>
  </body>
</template>
<script>
export default {
  head() {
    title: 'Todo Application - Login'
  },

  data() {
    return {
      form: {
        username: null,
        password: null,
      },
    }
  },

  methods: {
    async handleLogin(event) {
      event.preventDefault()
      let response = await this.$axios.post(
        'http://localhost:8000/api/user/login',
        {
          username: this.form.username,
          password: this.form.password,
        }
      )

      switch (response.data.message) {
        case 'LOGIN_SUCCESSFULL':
          this.$cookie.set('auth-token', response.data.token, {
            path: '/',
            maxAge: 14400,
          })
          return window.location.replace('/tasks')
        case 'USER_NOT_FOUND':
          alert('USER_NOT_FOUND')
        case 'INCORRECT_CREDENTIALS':
          alert('INCORRECT PASSWORD')
        default:
          return
      }
    },
  },
}
</script>
<style>
body {
  background-color: rgb(240, 240, 240);
}

.container {
  margin-top: 10%;
  background-color: white;
  width: 350px;
  height: fit-content;
  border-radius: 6px;
}

b-button {
  margin-top: 5px;
  margin-left: 5px;
}

p {
  margin-top: 5px;
}
</style>
