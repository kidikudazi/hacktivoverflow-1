<template>
  <v-content style="height:100%;">
    <v-container fluid fill-height>
      <v-layout align-center justify-center>
        <v-flex xs12 sm12 md12>
          <v-card class="elevation-12">
            <v-toolbar dark color="primary">
              <v-toolbar-title>Register</v-toolbar-title>
              <v-spacer></v-spacer>
            </v-toolbar>
            <v-card-text>
              <v-form>
                <v-text-field prepend-icon="person" name="name" label="name" type="text" v-model="registerForm.name"
                  :rules="[v => !!v || 'Name is required']"
                ></v-text-field>
                <v-text-field prepend-icon="alternate_email" name="Email" label="Email" type="text" v-model="registerForm.email"
                  :rules="[v => !!v || 'Email is required']"
                ></v-text-field>
                <v-text-field
                  id="password"
                  prepend-icon="lock"
                  name="password"
                  label="Password"
                  type="password"
                  v-model="registerForm.password"
                  :rules="[v => !!v || 'Password is required']"
                ></v-text-field>
              </v-form>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" @click.prevent="register">Register</v-btn>
            </v-card-actions>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script>
import swal from 'sweetalert'

export default {
  name: 'login-form',
  data: () => ({
    drawer: null,
    registerForm: {
      name: '',
      email: '',
      password: ''
    }
  }),
  props: {
    source: String,
  },
  methods: {
    register() {
      let { name, email, password } = this.registerForm
      axios({
        method: 'post',
        url: 'http://34.87.71.136/users/register',
        data: {
          name, email, password
        },
      })
        .then(({ data }) => {
          console.log(data)
          swal({
            type: 'success',
            title: 'Registered',
            text: `Welcome to the club, ${name}!`
          })
          this.$router.push('/')
        })
        .catch(({ response }) => {
          let { status, statusText, data } = response
          swal(`Error ${status}: ${statusText}`, data.message, 'error')
          console.log({ response })
        })
    }
  }
};
</script>
