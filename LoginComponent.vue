<template>
  <v-app id="inspire">
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col
            cols="12"
            sm="8"
            md="4"
          >
            <v-card class="elevation-12">
              <v-toolbar
                color="primary"
                dark
                flat
              >
                <v-toolbar-title>Login form</v-toolbar-title>
                <v-spacer />
                
              </v-toolbar>
              <v-card-text>
              <v-progress-linear
                :active="loading"
                :indeterminate="loading"
                absolute
                top
                color="deep-purple accent-4"
      ></v-progress-linear>
                <v-form>
                  <v-text-field
                    label="Login"
                    name="login"
                    v-model="email"
                    prepend-icon="mdi-account-circle-outline"
                    type="text"
                  />

                  <v-text-field
                    id="password"
                    label="Password"
                    name="password"
                    v-model="password"
                    prepend-icon="mdi-lock"
                    type="password"
                  />
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer />
                <v-btn color="primary" @click="login">Login</v-btn>
              </v-card-actions>
            </v-card>
            <v-snackbar
      v-model="snackbar"
    >
      {{ text }}
      <v-btn
        color="pink"
        text
        @click="snackbar = false"
      >
        Close
      </v-btn>
    </v-snackbar>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
  </template>
<script>
export default{
  data(){
    return {
      email:'',
      password:'',
      loading: false,
      snackbar:false,
      text:''
    }
  },
    methods:{
      login:function(){
  // Add a request interceptor
    axios.interceptors.request.use((config)=> {
    this.loading= true;
    return config;
  },(error)=> {
    this.loading=false;
    return Promise.reject(error);
  });

// Add a response interceptor
axios.interceptors.response.use((response) =>{
  // Any status code that lie within the range of 2xx cause this function to trigger
    this.loading=false;
    return response;
  },(error) => {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    this.loading=false;
    return Promise.reject(error);
  });
  axios.post('/api/login',{'email':this.email, 'password':this.password}) 
   .then(res =>{ localStorage.setItem('token',res.data.token)
    })
    .catch(err=> {
      this.text= err.response.data.status;
      this.snackbar=true;
      })
           
      }
    }
}</script>  
   
<style scoped></style>
