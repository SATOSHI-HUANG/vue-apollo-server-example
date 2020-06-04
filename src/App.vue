<template>
  <div id="app">
    <div>
      <h2>Search User</h2>
      <input type="text" v-model="inputName" />
      <p>ID: {{ user.id }}</p>
      <p>name: {{ user.name }}</p>
      <p>age: {{ user.age }}</p>
    </div>

    <div>
      <h2>Add User</h2>
      <input type="text" v-model="name" />
      <input type="text" v-model="age" />
      <button @click="addUser">Add User</button>
    </div>

    <div>
      <h2>User List</h2>
      <ul>
        <li v-for="user in users" :key="user.id">
          <p>ID: {{ user.id }}</p>
          <p>name: {{ user.name }}</p>
          <p>age: {{ user.age }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  name: "App",
  data() {
    return {
      users: [],
      user: {},
      inputName: "Fong",
      name: "",
      age: 0
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
    },
    users() {
      return {
        query: gql`
          {
            users {
              id
              name
              age
            }
          }
        `
      };
    }
  },
  methods: {
    addUser() {
      const name = this.name;
      const age = parseInt(this.age);

      this.$apollo
        .mutate({
          mutation: gql`
            mutation($name: String!, $age: Int!) {
              createUser(name: $name, age: $age) {
                id
                name
                age
              }
            }
          `,
          variables: {
            name,
            age
          }
        })
        .then(data => {
          console.log(data);
          this.users.push(data.data.createUser);
        })
        .catch(err => {
          throw err;
        });
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
