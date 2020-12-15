<template>
  <Navbar />
  <body>
    <table class="table table-striped">
      <thead>
        <tr class="bg-dark text-white">
          <td>ID</td>
          <td>Clase</td>
          <td>Dia</td>
          <td>Horario</td>
        </tr>
      </thead>
      <tbody v-if="clases.length > 0">
        <tr
          class="bg-info text-white"
          v-for="(clase, index) in clases"
          :key="index"
        >
          <td>{{ clase.codClase }}</td>
          <td>{{ clase.nombre_Clase }}</td>
          <td>{{ clase.dia }}</td>
          <td>{{ clase.hora }}</td>
        </tr>
      </tbody>
    </table>

    <!-- Botones bajo la tabla -->
    <!-- Boton Agregar  -->
    <div id="contenedor">
      <button
        class="favorite styled"
        type="button"
        @click="reservar = !reservar"
      >
        {{ reservar ? "No Reservar" : "Reservar" }}
      </button>

      
      <div v-if="reservar" class="alert alert-warning">
        <form novalidate autocomplete="off" @submit.prevent="enviar()">
          <div class="form-group">
            <label for="codClase">Escriba el codigo de la clase</label>
            <input
              type="number"
              id="codClase"
              class="form-control"
              v-model.number="v.f.codClase.$model"
            />
            <div
              v-if="v.f.codClase.$error && v.f.codClase.$dirty"
              class="alert alert-danger mt-1"
            >
              <div v-if="v.f.codClase.required.$invalid">
                Este campo es requerido
              </div>
              <div v-else-if="v.f.codClase.minLength.$invalid">
                El codigo de la clase debe tener al menos 3 caracteres
              </div>
            </div>
          </div>

          <div class="form-group">
            <label for="dni">Ingrese su DNI</label>
            <input
              type="number"
              id="dni"
              class="form-control"
              v-model="v.f.dni.$model"
            />
            <div v-if="v.f.dni.required.$invalid">Este campo es requerido</div>
            <div v-else-if="v.f.dni.minLength.$invalid">
              El dni debe tener al menos 8 caracteres
            </div>
            <div v-else-if="v.f.dni.maxLength.$invalid">
              El dni debe tener maximo 8 caracteres
            </div>
          </div>

          <div class="form-group">
            <router-link to="/confirmacion-turno">
              <button
                type="submit"
                :disabled="v.$invalid"
                class="favorite styled"
              >
                Confirmar
              </button>
            </router-link>
          </div>
        </form>

      

      </div>
    </div>
    <router-link to="/">
              <button
                
                class="favorite styled"
              >
                Volver
              </button>
            </router-link>
  </body>
</template>

<script>
import Navbar from "./Navbar.vue";
import { required, minLength, maxLength } from "@vuelidate/validators";
import { useVuelidate } from "@vuelidate/core";
export default {
  name: "src-components-turnos",
  props: [],
  components: {
    Navbar,
  },
  mounted() {
    if (!this.$store.state.isLoggedClient) {
      this.$router.push("/");
    }
    this.getDatosFormAxios();
  },
  created() {
    const rules = {
      f: {
        codClase: {
          required,
          minLength: minLength(3),
        },
        dni: {
          required,
          minLength: minLength(8),
          maxLength: maxLength(8),
        },
      },
    };
    const f = this.f;
    this.v = useVuelidate(rules, { f });
  },
  data() {
    return {
      f: {
        codClase: "",
        dni: "",
      },
      v: null,
      reservar: false,
      url: "http://localhost:3000/api/clases/",
      urlAnotarse: "http://localhost:3000/api/reservas/",
      clases: [],
    };
  },
  methods: {
    //anotarse(codClase){
    //console.log("anotarse",codClase)
    //let usuarioConectado = JSON.parse(sessionStorage.getItem('client'))
    // hay que leer de vuex
    //this.$store.state
    //console.log(codClase, usuarioConectado)
    //this.postReservaUsuario(codClase,usuarioConectado.dni)

    //},
    getDatosFormAxios() {
      this.axios(this.url)
        .then((resp) => {
          console.log(resp.data);
          this.clases = resp.data;
        })
        .catch((error) => console.log("HTTP GET ERROR", error));
    },
    //postReservaUsuario(codClase,usuarioConectado) {
    //      this.axios.post(this.urlAnotarse, codClase , usuarioConectado.dni, {'content-type':'application/json'})
    //      .then(resp => {
    //        console.log(resp.data)
    //
    //      })
    //      .catch(error => console.log('HTTP POST ERROR', error))
    //  }
    //},
    sendDatosFormAxios(data) {
      this.axios
        .post(this.urlAnotarse, data, { "content-type": "application/json" })
        .then((resp) => {
          console.log(resp.data);
          this.getDatosFormAxios();
        })
        .catch((error) => {
          console.log(error);
          this.error = true;
        });
    },
    enviar() {
      this.v.$touch();
      if (!this.v.$invalid) {
        let form = this.f;
        console.log(form);
        this.sendDatosFormAxios(form);
        this.resetForm();
        this.v.$reset();
        this.getDatosFormAxios();
      }
    },
    resetForm() {
      this.v.f.codClase.$model = "";
      this.v.f.dni.$model = "";
    },
    computed: {},
  },
};
</script>

<style scoped lang="css">
.styled {
  width: 200px;
  border: 0;
  line-height: 2.5;
  padding: 0 20px;
  font-size: 1.2rem;
  text-align: center;
  color: #fff;
  margin-left: 50%;
  transform: translateX(-50%);
  text-shadow: 1px 1px 1px #000;
  border-radius: 10px;
  background-color: rgb(3, 119, 28);
  background-image: linear-gradient(
    to top left,
    rgba(0, 0, 0, 0.2),
    rgba(0, 0, 0, 0.2) 30%,
    rgba(0, 0, 0, 0)
  );
  box-shadow: inset 2px 2px 3px rgba(255, 255, 255, 0.6),
    inset -2px -2px 3px rgba(0, 0, 0, 0.6);
}

.styled:hover {
  background-color: rgb(6, 180, 6);
}

.styled:active {
  box-shadow: inset -2px -2px 3px rgba(255, 255, 255, 0.6),
    inset 2px 2px 3px rgba(0, 0, 0, 0.6);
}

.boton {
  background-color: #000;
}

.contenedor {
  width: 33.33%;
}
</style>
