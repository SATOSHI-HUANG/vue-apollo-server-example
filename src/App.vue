<template>
  <div id="app">
    <input v-model="inputName" type="text" />
    <p>ID: {{ user.id }}</p>
    <p>Name: {{ user.name }}</p>
    <p>Age: {{ user.age }}</p>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "App",
  data() {
    return {
      user: [],
      inputName: "Fong"
    };
  },
  apollo: {
    user() {
      return {
        query: gql`
          query($name: String!) {
            user(name: $name) {
              id
              name
              age
            }
          }
        `,
        variables() {
          return {
            name: this.inputName
          };
        }
      };
    }
  }
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
