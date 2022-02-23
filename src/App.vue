<template>
  <h1 id="title">Liste des utilisateurs de mon super APi</h1>
  <ol>
    <!-- 
        vf for permet de parcourir chaque instance de mon tableau 
        et de l'afficher dans la liste.       
      -->
    <li v-for="user in users" :key="user.id">
      <h6>{{ user.name }} | {{ user.email }} > <a href="">Supprimer</a></h6>
    </li>

    <!-- 
      - Notre formulaire a traité en vuejs
      - v-model permet de lier la valeur de l'input a celle de la variable au niveau de vue.
      - @submit.prevent permet de soumettre le formulaire en appeelant une methode vue.
    -->
    <form method="post" @submit.prevent="registerUser">
      <p>{{ register.name }}</p>
      <input
        type="text"
        name="name"
        v-model="register.name"
        placeholder="Nom"
      /><br />
      <input
        type="text"
        name="email"
        v-model="register.email"
        placeholder="Email"
      /><br />
      <button type="submit">S'inscrire</button>
    </form>
  </ol>
</template>
<script>

// axios librairie pour faire les requetes HTTP
import axios from "axios";

export default {
  name: "App",
  components: {},

//Declaration des variables(data)
  data() {
    return {
      users: {},
      locations: {},
      register: {
        name: "",
        email: "",
      },

      success: "",
    };
  },

  // Methode appelée quant le composant est chargé(page)
  mounted() {
     axios
      .get("http://localhost:8000/api/users")
      .then((response) => {
        this.users = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },

  //Method pour faire les appeles axios en POST pour sauvegarder des données
  methods: {
    async registerUser() {
     await axios
        .post("http://localhost:8000/api/users/create", this.register)
        .then((response) => {
          this.users.push(response.data);
          this.register.name = "";
          this.register.email = "";
        })
        .catch((resposne) => {
          console.log(resposne);
        });
    },
  },
};
</script>

<style>
#app {
  margin-left: auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 20px;
  font-weight: bold;
}

ul {
  list-style: none;
  display: flex;
  text-align: center;
  justify-content: center;
}

ul,
li {
  margin: 10px;
  font-size: 20px;
  color: gray;
}

a {
  color: red;
  text-decoration: none;
}

a:hover {
  color: #fff;
}

#title {
  text-align: center;
  font-size: 30px;
}
</style>
