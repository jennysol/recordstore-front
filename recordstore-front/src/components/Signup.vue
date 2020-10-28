<template>
<div 
  class="page"
  :style="{ backgroundImage: 'url(' + require('@/assets/microfone.jpg')+  ')' }"

>
  <div class="container-form">
    <h3 class="title">Sign Up</h3>
    <form @submit.prevent="signup">
      <div class="text-red" v-if="error">{{ error }}</div>

      <div class="form">
        <label for="email" class="label">E-mail</label>
        <input type="email" v-model="email" class="input" id="email" placeholder="andy@web-crunch.com"><br/>

        <label for="password" class="label">Password</label>
        <input type="password" v-model="password" class="input" id="password" placeholder="Password"><br/>

        <label for="password_confirmation" class="label">Password Confirmation</label>
        <input type="password" v-model="password_confirmation" class="input" id="password_confirmation" placeholder="Password Confirmation"><br/>
      </div>
      <button type="submit" class="button-signup">Sign Up</button>

      <div class="my-4"><router-link to="/" class="link">Sign In</router-link></div>
    </form>
  </div>
</div>
</template>

<script>
export default {
name: 'Signup',
data () {
  return {
    email: '',
    password: '',
    password_confirmation: '',
    error: ''
  }
},
created () {
  this.checkedSignedIn()
},
updated () {
  this.checkedSignedIn()
},
methods: {
  signup () {
    this.$http.plain.post('/signup', { email: this.email, password: this.password, password_confirmation: this.password_confirmation })
      .then(response => this.signupSuccessful(response))
      .catch(error => this.signupFailed(error))
  },
  signupSuccessful (response) {
    if (!response.data.csrf) {
      this.signupFailed(response)
      return
    }
    localStorage.csrf = response.data.csrf
    localStorage.signedIn = true
    this.error = ''
    this.$router.replace('/records')
  },
  signupFailed (error) {
    this.error = (error.response && error.response.data && error.response.data.error) || 'Something went wrong'
    delete localStorage.csrf
    delete localStorage.signedIn
  },
  checkedSignedIn () {
    if (localStorage.signedIn) {
      this.$router.replace('/records')
    }
  }
}
}
</script>

<style scoped>

.page{
  width: 100vw;
  height: 100vh;

  display: flex;
  flex-direction: column;

  background: #fff;
  font-family: 'Poppins', sans-serif;
  color:#5a189a

}

.container-form {
  width: 100%;
  height: 100%;

  width: 450px;
  height: 100vh;

  background-color: #fff;
  
  display: flex;
  flex-direction: column;

  align-items: center;
  justify-content: center;
}

.title {
color: #5a189a;
font-weight: 600;
font-size: 25px;
margin: 20px;
}

.label {
  font-family: 'Poppins', sans-serif;
  font-weight: 400px;
  color: #555b6e;
}
.form {
  width: 300px;
  display: flex;
  flex-direction: column;
}

.input {
  border-radius: 10px;
  padding: 16px;
  width: 100%;

  border: 2px solid #9d4edd;

  display: flex;
  align-items: center;

  margin-top: 10px;

  background: transparent;
}
.input:hover{
  background-color: #f5f3f4;
  border-color: #7b2cbf;

}

.button-signup {
  border-radius: 10px;
  border: none;
  background-color: #5a189a;
  color: #fff;

  padding: 10px;
}
.button-signup:hover{
  background-color:#7b2cbf;
}

.link {
  position: relative;
  top: 10px;

  text-decoration: none;
  transition: color 0.2s;
  color: #5a189a;
}
.link:hover {
  color: #7b2cbf;
}

</style>