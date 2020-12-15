<template>
<header>
    <h1><span> One More</span> Gym</h1>
</header>


<body>
    

   <div class="login-page">
  <div class="form">

                <!-- CUSTOMER LOGIN FORM -->
                <form novalidate autocomplete="off" class="login-form" @submit.prevent="send()">
                    <h4 >Iniciar Sesión Administrador</h4>
                    <hr>
                    <div v-if="error" class="alert alert-danger">
                        Los datos ingresados son incorrectos.
                    </div>
                    <div class="form-group col-12">
                        <label for="email">Correo electrónico</label>
                        <input type="text" id="email" class="form-control" v-model="$v.f.email.$model">
                        <div v-if="$v.f.email.$error && $v.f.email.$dirty" class="alert alert-danger mt-1">
                            <div v-if="$v.f.email.required.$invalid">- Este campo es requerido</div>
                            <div v-if="$v.f.email.email.$invalid">- Debe ser un email válido</div>
                        </div>
                    </div>
                    <div class="form-group  col-12">
                        <label for="password">Contraseña</label>
                        <input type="password" id="password" class="form-control" v-model="$v.f.password.$model">
                        <div v-if="$v.f.password.$error && $v.f.password.$dirty" class="alert alert-danger mt-1">
                            <div v-if="$v.f.password.required.$invalid">- Este campo es requerido</div>

                        </div>
                    </div>
                    <br>
                    <div class="form-group">
                        <button type="submit" :disabled="false"  value="Enviar"> Ingresar
                        </button>
                   </div>

                    <br>
                    <div class="form-group">
                    <router-link to="/">
                        <button > Volver
                        </button>
                        </router-link>
                   </div>

                </form>
                
                <br>
                
            </div>
        </div>
    
</body>
</template>

<script>
  import {required,email} from '@vuelidate/validators'
  
  export default  {
    name: 'src-components-login',
    props: [],
    mounted () {
       if (this.$store.state.isLoggedAdmin) {
            this.$router.push('/home-adm')
        }
        
        this.resetForm()
    },
    data () {
      return {
        f: this.resetForm(),
        url: 'http://localhost:3000/api/admins/login',
      }
    },
    validations: {
        f: {
            email: {
                required,
                email
            },
            password: {
                required
            }
        }
    },
    methods: {    
      sendDataFormAxios(data) {
            this.axios.post(this.url, data, {
                    'content-type': 'application/json'
                })
                .then(res => {
                    if (res.data) {
                        //let userCompleto = res.data.user
                        //let userCompletoSTR = JSON.stringify(userCompleto)
                        //sessionStorage.setItem('admin', res.data.username)
                        //this.$store.dispatch('getStatusAdmin', res.data.username);
                        //this.$router.push('/home-adm')


                        let userCompleto = res.data.user
                        let userCompletoSTR = JSON.stringify(userCompleto)
                        sessionStorage.setItem('admin', userCompletoSTR)
                        this.$store.dispatch('getStatusAdmin', userCompletoSTR);
                        this.$router.push('/home-adm')
                    }
                })
                .catch(error => {
                    console.log(error)
                    this.error = true
                    this.loading = false
                    setTimeout(() => {
                        this.error = false
                    }, 5000)
                })
        },
        send() {
            this.$v.$touch()
            if (!this.$v.$invalid) {
                let form = {
                    email: this.$v.f.email.$model,
                    password: this.$v.f.password.$model,
                }
                this.loading = true
                this.sendDataFormAxios(form)
            }
        },
        resetForm() {
            return {
                email: '',
                password: ''
            }
        }
        }, 
    computed: {

    }
}


</script>

<style scoped lang="css">
  @import url(https://fonts.googleapis.com/css?family=Roboto:300);

.login-page {
  width: 360px;
  padding: 8% 0 0;
  margin: auto;
}
.form {
  position: relative;
  z-index: 0;
  max-width: 360px;
  margin: 0 auto 100px;
  padding: 45px;
  text-align: center;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
  background: #c6f3ae; /* fallback for old browsers */
  background: -webkit-linear-gradient(right, #82ca5b, #8DC26F);
  background: -moz-linear-gradient(right, #9eeb74, #8DC26F);
  background: -o-linear-gradient(right, #93df6a, #8DC26F);
  background: linear-gradient(to left, #86ce60, #8DC26F);
}
.form input {
  font-family: "Roboto", sans-serif;
  outline: 0;
  background: #f2f2f2;
  width: 100%;
  border: 0;
  margin: 0 0 15px;
  padding: 15px;
  box-sizing: border-box;
  font-size: 14px;
}
.form button {
  font-family: "Roboto", sans-serif;
  text-transform: uppercase;
  outline: 0;
  background: #4CAF50;
  width: 100%;
  border: 0;
  padding: 15px;
  color: #FFFFFF;
  font-size: 14px;
  -webkit-transition: all 0.3 ease;
  transition: all 0.3 ease;
  cursor: pointer;
}

.form button:hover,.form button:active,.form button:focus {
  background: #43A047;
}

body {
  font-family: "Roboto", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width : 100%;
  height: 100%;      
}

header{
    background-image: url(./images/hero.jpg);
    color: #f4f4f4;
}

header img{
    float: left;
    display: inline-block;
}

header h1{
    font-size: 40px; 
    color: #f4f4f4;
    display: inline-block;
    position: relative;
    padding: 20px 20px 0 0;
    display: inline-block;
}
</style>
