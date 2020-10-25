<template>
  <div 
    class="page"
    :style="{ backgroundImage: 'url(' + require('@/assets/image.jpg')+  ')' }"
  >
    <div class="container-form">
      <h3 class="title">Sign In</h3><br/>
      <form clas="form-center" @submit.prevent="signin">
        <div class="text-red" v-if="error">{{ error }}</div>

          <label for="email" class="label">E-mail</label>
          <input type="email" v-model="email" class="input" id="email" placeholder="any@gmail.com">
          <br/>
          <label for="password" class="label">Password</label>
          <input type="password" v-model="password" class="input" id="password" placeholder="Password">
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
.page {
  width: 100vw;
  height: 100vh;

  background: no-repeat right top fixed;
  color: #4361ee;
  font-family: 'Poppins', sans-serif;

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

.container-form {
  background: #fff;
  width: 450px;
  height: 100vh;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  margin-top: 0px;
}

.input {
  border-radius: 10px;
  padding: 16px;
  width: 100%;

  border: 2px solid #4895ef;

  display: flex;
  align-items: center;

  margin-top: 10px;

  background: transparent;
  
}

.input:hover{
  background-color: #f5f3f4;
  border-color: #3f37c9;

}

.button {
  position: relative;
  left: 130px;
  top: 20px;

  border-radius: 10px;
  border-color: #4361ee;
  background-color: #4361ee;
  border-width: 0.5px;
  padding: 8px;

  color: #fff;
}

.button:hover{
  background-color:#3f37c9;
}

.link {
  text-decoration: none;
  transition: color 0.2s;
  color: #4361ee;;
}
.link:hover {
  color: #3f37c9;
}


</style>
