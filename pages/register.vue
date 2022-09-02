<template>
  <body>
    <b-container
      class="container shadow p-3 mb-5 bg-white rounded"
      id="container-div"
    >
      <div>
        <b-form @submit="handleRegister">
          <legend>Register</legend>
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
            id="email-input-group"
            label="E-mail Address"
            label-for="pwd-input"
          >
            <b-form-input
              id="email-input"
              v-model="form.email"
              type="email"
              placeholder="E-mail"
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
            <b-button type="submit" variant="primary">Create</b-button>
            <a href="../">
              <b-button type="button" variant="danger">Back</b-button>
            </a>
          </b-form-group>
        </b-form>
      </div>
      <div>
        <b-alert
          variant="success"
          v-if="alerts.success.message"
          v-html="alerts.success.message"
        >
        </b-alert>
        <b-alert
          variant="danger"
          v-if="alerts.error.state"
          v-html="alerts.error.message"
          show
        >
        </b-alert>
        <b-alert
          variant="warning"
          v-if="alerts.warning.state"
          v-html="alerts.warning.message"
          show
        >
        </b-alert>
      </div>
    </b-container>
  </body>
</template>
<script>
export default {
  head() {
    return {
      title: 'Todo Application - Register',
    }
  },

  data() {
    return {
      form: {
        username: null,
        email: null,
        password: null,
      },

      alerts: {
        success: {
          state: false,
          message: null,
        },
        warning: {
          state: false,
          message: null,
        },
        error: {
          state: false,
          message: null,
        },
      },
    }
  },

  methods: {
    async handleRegister(event) {
      event.preventDefault()
      let response = await this.$axios.post('http://localhost:8000/api/user/', {
        username: this.form.username,
        email: this.form.email,
        password: this.form.password,
      })

      this.alerts.success.state = false
      this.alerts.warning.state = false
      this.alerts.error.state = false

      switch (response.data.message) {
        case 'ACCOUNT_CREATED':
          this.alerts.success.message =
            '<strong>Your account was successfully created!</strong> You can log-in <a href="../">here</a>.'
          this.alerts.success.state = true
          return
        case 'USER_ALREADY_EXISTS':
        case 'EMAIL_ALREADY_EXISTS':
          this.alerts.warning.message =
            'The provided <strong>user or email already exist</strong>.'
          this.alerts.warning.state = true
          return
        default:
          this.alerts.error.message =
            'An unexpected error occurred, <strong>contact the webmaster as soon as possible</strong>!'
          this.alerts.error.state = true
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
</style>
