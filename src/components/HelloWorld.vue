<template>
  <div class="hello">
    <h1>{{ blague }}</h1>
    <div v-for="(user, key) in users" :key="key">
      <ul>
        <!-- <li>{{ user.name }}</li> -->
      </ul>
    </div>
  </div>
  <button @click="getJoke()">
    Oh, you want a joke ? Well then, click here !
  </button>
  <!---------- Gestion HTTP POST ------->
  <h1>HTTP POST</h1>
  <div>
    <label>NOM</label>
    <input v-model="userNom" />
  </div>
  <div>
    <label>PRENOM</label>
    <input v-model="userPrenom" />
  </div>
  <div>
    <label>ADRESSE</label>
    <input v-model="userAdresse" />
  </div>
  <button @click="submit()">Submit</button>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  data() {
    return {
      blague: "",
      users: [],
      userNom: "",
      userPrenom: "",
      userAdresse: "",
    };
  },
  props: {
    msg: String,
  },
  methods: {
    getJoke() {
      axios.get("https://api.chucknorris.io/jokes/random").then((data) => {
        console.log(data.data);
        this.blague = data.data.value;
      });
    },
    submit() {
      axios.defaults.headers.post["Content-Type"] =
        "application/x-www-form-urlencoded";
      axios
        .post("http://localhost:8888/form.php", "nom=" + this.userNom + '&prenom=' + this.userPrenom + '&adresse=' + this.userAdresse)
        .then((response) => {
          console.log(response);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },

  mounted() {
    this.getJoke();
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((apiUser) => {
        console.log(apiUser.data);
        this.users = apiUser.data;
      })
      .catch((error) => {
        console.log("Oups, erreur", error);
      });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>