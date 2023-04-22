<template>
  <div class="container">
    <form v-on:submit.prevent="submitForm">
      <div class="form-group">
        <label for="username">Username</label>
        <input type="text" class="form-control" id="username" v-model="user.username" required>
      </div>
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" class="form-control" id="email" v-model="user.email" required>
      </div>
      <div class="form-group">
        <label for="password1">Password</label>
        <input type="password" class="form-control" id="password1" v-model="user.password1" required>
      </div>
      <div class="form-group">
        <label for="password2">Confirm Password</label>
        <input type="password" class="form-control" id="password2" v-model="user.password2" required>
      </div>
      <button type="submit" class="btn btn-primary">Register</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'UserForm',
  data() {
    return {
      user: {
        username: '',
        email: '',
        password1: '',
        password2: ''
      },
      errors: []
    };
  },
  methods: {
    submitForm() {
          console.log('submitForm')

          this.errors = []

          if (this.user.username === '') {
              this.errors.push('The username is missing!')
          }

          if (this.user.password1 === '') {
              this.errors.push('The password is missing!')
          }

          if (this.user.password1 !== this.user.password2) {
              this.errors.push('The passwords are not matching!')
          }

          if (this.user.password1.length < 8) {
              this.errors.push('The password is too short!')
          }

          const commonPasswords = ['123456', 'password', '123456789', '12345678', '12345', '1234567', '1234567890', 'letmein', '1234', 'football', 'iloveyou', 'admin', 'welcome', 'monkey', 'login', 'abc123', 'starwars', '123123', 'dragon', 'passw0rd']

          if (commonPasswords.includes(this.user.password1)) {
              this.errors.push('This password is too common!')
          }

          if (!this.errors.length) {
              const formData = {
                  username: this.user.username,
                  email: this.user.email,
                  password: this.user.password1
              }

              axios
                  .post('http://127.0.0.1:8000/api/users/', formData)
                  .then(response => {
                    console.log(response)
                      this.$router.push('/login')
                  })
                  .catch(error => {
                      if (error.response) {
                          for (const property in error.response.data) {
                              this.errors.push(`${property}: ${error.response.data[property]}`)
                          }

                          console.log(JSON.stringify(error.response.data))
                      } else if (error.message) {
                          this.errors.push('Something went wrong. Please try again')
                          
                          console.log(JSON.stringify(error))
                      }
                  })
          }
      }
  }
};
</script>
