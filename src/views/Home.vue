import unfetch from 'unfetch';
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
  </div>
  <div class="container-sm">
    <div>
      <!-- Button trigger modal -->
      <button
        type="button"
        class="btn btn-primary"
        data-bs-toggle="modal"
        data-bs-target="#exampleModal"
      >
        Crear Usuario
      </button>

      <!-- Modal -->
      <div
        class="modal fade"
        id="exampleModal"
        tabindex="-1"
        aria-labelledby="exampleModalLabel"
        aria-hidden="true"
      >
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
              <button
                type="button"
                class="btn-close"
                data-bs-dismiss="modal"
                aria-label="Close"
              ></button>
            </div>
            <div class="modal-body">
              <form>
                <div class="mb-3">
                  <label for="exampleInputEmail1" class="form-label"
                    >NOMBRE</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="exampleInputEmail1"
                    v-model="nombreUsu"
                    @keyup.enter="postUser"
                  />
                </div>
                <div class="mb-3">
                  <label for="exampleInputPassword1" class="form-label"
                    >APELLIDO</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="exampleInputPassword1"
                    v-model="apellidoUsu"
                    @keyup.enter="postUser"
                  />
                </div>
                <div class="mb-3">
                  <label for="exampleInputPassword1" class="form-label"
                    >EDAD</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="exampleInputPassword1"
                    v-model="edadUsu"
                    @keyup.enter="postUser"
                  />
                </div>
                <div class="mb-3">
                  <label for="exampleInputPassword1" class="form-label"
                    >CELULAR</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="exampleInputPassword1"
                    v-model="celularUsu"
                    @keyup.enter="postUser"
                  />
                </div>
                <div class="mb-3">
                  <label for="exampleInputPassword1" class="form-label"
                    >DNI</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="exampleInputPassword1"
                    v-model="dniUsu"
                    @keyup.enter="postUser"
                  />
                </div>
                <div class="mb-3">
                  <label for="exampleInputPassword1" class="form-label"
                    >ESTADO CIVIL</label
                  >
                  <input
                    type="text"
                    class="form-control"
                    id="exampleInputPassword1"
                    v-model="estadoUsu"
                    @keyup.enter="postUser"
                  />
                </div>

                <button type="submit" @click="postUser" class="btn btn-primary">
                  Submit
                </button>
              </form>
            </div>
            <div class="modal-footer">
              <button
                type="button"
                class="btn btn-secondary"
                data-bs-dismiss="modal"
              >
                Close
              </button>
              <button type="button" class="btn btn-primary">
                Save changes
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <table class="table table-success">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Nombre</th>
          <th scope="col">Apellido</th>
          <th scope="col">Accion</th>
        </tr>
      </thead>
      <tbody v-for="(usuario, index) in usuarios" :key="index">
        <tr>
          <th scope="row">{{ usuario.id }}</th>
          <td>{{ usuario.nombre }}</td>
          <td>{{ usuario.apellido }}</td>
          <td>
            <button
              type="button"
              click="eliminarUsu"
              class="btn btn-danger"
              @click="eliminarUsu"
              v-on:click="eliminarUsu(usuario.id)"
            >
              Eliminar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
  data() {
    return {
      usuarios: [],
      maximo: 500,
      minimo: 0,
      users: {
        nombreUsu: "",
        apellidoUsu: "",
        edadUsu: "",
        celularUsu: "",
        dniUsu: "",
        estadoUsu: "",
      },
    };
  },
  methods: {
    async getUser() {
      const data = await fetch("http://localhost:3000/users");
      const info = await data.json();
      this.usuarios = info;
    },

    async postUser(e) {
      fetch("http://localhost:3000/users", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          // your expected POST request payload goes here
          id: Math.floor(
            Math.random() * (this.maximo + 1 - this.minimo) + this.minimo
          ),
          nombre: this.nombreUsu,
          apellido: this.apellidoUsu,
          edad: Number(this.edadUsu),
          celular: Number(this.celularUsu),
          dni: Number(this.dniUsu),
          estadoCivil: this.estadoUsu,
        }),
      });
    },
    async eliminarUsu(id) {
      fetch("http://localhost:3000/users/" + id, {
        method: "DELETE",
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then((res) => res.json())
        .then((data) => {
          // enter you logic when the fetch is successful
          console.log(data);
        })
        .catch((error) => {
          // enter your logic for when there is an error (ex. error toast)
          console.log(error);
        });
    },
  },
  created() {
    this.getUser();
    this.eliminarUsu();
  },
};
</script>
