<template>
  <div>
    <form>
      <label for="name">name</label><br />
      <input type="text" id="name" name="name" v-model="userName" /><br />
      <label for="age">age</label><br />
      <input type="text" id="age" name="age" v-model="userAge" /><br /><br />

      <button @click.prevent="saveUser()">submit</button>
      <button @click.prevent="saveUpdateUser()">update</button>
    </form>
    <!-- {{ userName}} {{userAge}} -->
    <table>
      <tr>
        <th>id</th>
        <th>name</th>
        <th>age</th>
        <th>edit</th>
        <th>delete</th>
      </tr>
      <tr v-for="(user, index) in users" v-bind:key="index">
        <td>{{ user.id }}</td>
        <td>{{ user.name }}</td>
        <td>{{ user.age }}</td>
        <td><button @click.prevent="updateUser(user)">edit</button></td>
        <td><button @click.prevent="deleteUser(user.id)">delete</button></td>
      </tr>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      users: [],
      userName: "",
      userAge: null,
      userId: null,
    };
  },

  mounted() {
    this.getUsers();
  },

  methods: {
    getUsers() {
      axios
        .get("http://localhost:3000/posts/get-all-posts")
        .then((response) => {
          console.log(response.data);
          this.users = response.data;
        });
    },

    updateUser(user) {
      console.log(user);
      this.userId = user.id;
      this.userName = user.name;
      this.userAge = user.age;
    },

    saveUpdateUser() {
      const saveUser = {
        id: this.userId,
        name: this.userName,
        age: this.userAge,
      };
      axios
        .post("http://localhost:3000/posts/update-post", saveUser)
        .then((response) => {
          console.log(response);
          this.getUsers();
        });
    },

    saveUser() {
      //   alert(1);
      const user = {
        name: this.userName,
        age: this.userAge,
      };
      let url = "http://localhost:3000/posts/add-new-post";
      console.log(user);
      axios.post(url, user).then((response) => {
        console.log(response);
        this.getUsers();
      });
    },

    deleteUser(userId) {
      // alert (userId);
      const user = {
        id: userId,
      };
      axios
        .post("http://localhost:3000/posts/delete-post", user)
        .then((response) => {
          console.log(response);
          this.getUsers();
        });
    },
  },
};
</script>

<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>


