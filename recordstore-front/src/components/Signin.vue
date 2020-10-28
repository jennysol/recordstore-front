<template>
  <div 
    class="page"
    :style="{ backgroundImage: 'url(' + require('@/assets/image.jpg')+  ')' }"
  >
    <div class="container-form">
      <h3 class="title">Sign In</h3><br/>
      <form clas="form-center" @submit.prevent="signin">
        <div class="text-red" v-if="error">{{ error }}</div>

        <div class="form">
          <label for="email" class="label">E-mail</label>
          <input type="email" v-model="email" class="input" id="email" placeholder="any@gmail.com">
          <br/>
          <label for="password" class="label">Password</label>
          <input type="password" v-model="password" class="input" id="password" placeholder="Password"><br/>
        </div>
        <button type="submit" class="button">Sign In</button>

        <div class="form-3"><router-link to="/signup" class="link">Sign up</router-link></div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Signin',
  data () {
    return {
      email: '',
      password: '',
      error: ''
    }
  },
  created () {
    this.checkSignedIn()
  },
  updated () {
    this.checkSignedIn()
  },
  methods: {
    signin () {
      this.$http.plain.post('/signin', { email: this.email, password: this.password })
        .then(response => this.signinSuccessful(response))
        .catch(error => this.signinFailed(error))
    },
    signinSuccessful (response) {
      if (!response.data.csrf) {
        this.signinFailed(response)
        return
      }
      localStorage.csrf = response.data.csrf
      localStorage.signedIn = true
      this.error = ''
      this.$router.replace('/records')
    },
    signinFailed (error) {
      this.error = (error.response && error.response.data && error.response.data.error) || ''
      delete localStorage.csrf
      delete localStorage.signedIn
    },
    checkSignedIn () {
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
  width: 450px;
  height: 100vh;

  background-color: #fff;
  
  display: flex;
  flex-direction: column;

  align-items: center;
  justify-content: center;
}

.form {
  width: 300px;
  display: flex;
  flex-direction: column;
}

.title {
  font-family: 'Poppins', sans-serif;
  font-weight: 600;
  font-size: 28px;

  margin: 20px;
}

.label {
  font-family: 'Poppins', sans-serif;
  font-weight: 400px;
  color: #555b6e;
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

.button {
  border-radius: 10px;
  border: none;
  background-color: #5a189a;
  color: #fff;

  padding: 10px;
}
.button:hover{
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

/* @media only screen and (max-width: 600px) {
  .page{
    background: #fff;
    background-image: none;
  }
} */


</style>
