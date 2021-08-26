<template>
  <Header />
  <div class="container">
    <div class="row p-5 text-center">
      <div class="form-group" v-if="showForm">
        <h1 for="color">Choose the color</h1>
        <select
          v-model="color"
          id="color"
          class="form-control"
          @change="handleChange($event)"
        >
          <option
            :value="mycolor.name"
            v-for="mycolor in colors"
            :key="mycolor.name"
          >
            {{ mycolor.name }}
          </option>
        </select>
      </div>
    </div>
    <div class="col-md-6 text-right ml-3">
      <button
        @click.prevent="displayForm"
        class="btn btn-sm"
        :class="{ 'btn-success': !showForm, 'btn-danger': showForm }"
      >
        {{ showForm ? "Hide Result" : "Show Result" }}
      </button>
    </div>

    <div class="row p-5" v-if="showForm" ref="couleur">
      <div class="col-md-4" v-for="(title, index) in titles" :key="index">
        <div class="card flex-row">
          <div>
            <h3 class="card-title">{{ title }}</h3>
          </div>
          <div class="d1">
            <button
              id="icon"
              class="btn-close bg-danger"
              @click="deleteWord(index)"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- ptransporter des elements  d'un div vers une autre -->
  <teleport to="#alert" v-if="wordDeleted">
    <div class="alert alert-danger text-center">
      <strong>Word deleted </strong>
    </div>
  </teleport>
  <img class="logo" alt="Vue logo" src="./assets/logo.jpg" />
</template>

<script>
import Header from "./components/Header.vue";

import axios from "axios";
export default {
  name: "App",
  components: {
    Header,
  },
  //data definition
  data() {
    return {
      titles: [],
      wordDeleted: false,
      colors: [
        { name: "red" },
        { name: "yellow" },
        { name: "blue" },
        { name: "white" },
      ],
      color: "white",
      showForm: true,
    };
  },
  //call api when page created
  created() {
    this.getPayload();
  },
  methods: {
    //get data from api
    getPayload() {
      console.log("gere");
      axios
        .get("https://jsonplaceholder.typicode.com/todos")
        .then((res) => {
          this.titles = res.data[0].title.split(" ");
          //console.log("data",this.titles)
        })
        .catch((err) => console.log(err));
    },
    //delete item from table
    deleteWord(id) {
      if (confirm("are you sure about deleting item")) {
        this.titles.splice(id, 1);
        this.wordDeleted = true;
        setTimeout(() => {
          this.wordDeleted = false;
        }, 3000);
      }
    },
    //change div color when change select value
    handleChange(event) {
      this.$refs.couleur.setAttribute(
        "style",
        "background-color:" + event.target.value
      );
    },
    //show or hide form
    displayForm() {
      this.showForm = !this.showForm;
    },
  },
};
</script>

<style scoped>
.logo {
  width: 200px;
  height: 100px;
  position: absolute;
  bottom: 40px;
  right: 20px;
}
#icon {
  cursor: "pointer";
  width: 30px;
  height: 30px;
}
.d1 {
  margin-left: auto;
}
</style>
