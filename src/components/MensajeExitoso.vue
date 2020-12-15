<template>
  
  <body>
    

  <div class="card">
      <div style="border-radius:200px; height:200px; width:200px; background: #F8FAF5; margin:0 auto;">
        <i class="checkmark">✓</i>
      </div>
        <h1>Te anotaste exitosamente a la clase</h1>
        <h2>Su codigo de registro es {{getRandom(10000,999999999)}} </h2> 
        <router-link to="/turnos">
                    <button 
                    class="btn btn-warning m-3"> <p>Volver a la lista de clases!</p>
                    </button> 
                    </router-link>
      </div>
    </body>
  

</template>

<script>

  export default  {
    name: 'src-components-mensaje-exitoso',
    props: [],
    mounted () {
      if (!this.$store.state.isLoggedClient){
            this.$router.push('/')
        }
    },
    data () {
      return {

      }
    },
    methods: {

  getRandom(min,max) {
  return  Math.floor(Math.random() * (max - min)) + min;
}
},

    computed: {

    }
}


</script>

<style scoped lang="css">
     body {
        text-align: center;
        padding: 40px 0;
        background: #EBF0F5;
      }
        h1 {
          color: #88B04B;
          font-family: "Nunito Sans", "Helvetica Neue", sans-serif;
          font-weight: 900;
          font-size: 40px;
          margin-bottom: 10px;
        }
        h2{

          color: #89acda;
          font-family: "Nunito Sans", "Helvetica Neue", sans-serif;
          font-weight: 800;
          font-size: 30px;
          margin-bottom: 10px;

        }

        p {
          color: #404F5E;
          font-family: "Nunito Sans", "Helvetica Neue", sans-serif;
          font-size:20px;
          margin: 0;
        }
      i {
        color: #9ABC66;
        font-size: 100px;
        line-height: 200px;
        margin-left:-15px;
      }
      .card {
        background: white;
        padding: 60px;
        border-radius: 4px;
        box-shadow: 0 2px 3px #C8D0D8;
        display: inline-block;
        margin: 0 auto;
      }
</style>
