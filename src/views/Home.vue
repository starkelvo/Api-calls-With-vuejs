<template>
  <div class="user">
    <b-container>
      <div class="header">
        <!-- <h1 style="text-align: center">List of Users</h1> -->
        <div style="text-align: center">
      
            <b-form-input
          id="input-1"
          v-model="name"
          type="name"
          required
          placeholder="Enter New User"
       style="width:600px;" ></b-form-input>

          <b-button variant="success" @click="update()" style="margin-top:10px;">Add New User</b-button>
        </div>
      </div>
      <div style="margin-top: 10px">
        <b-list-group>
          <h1 style="color:white">List of Users</h1>
          <b-list-group-item v-for="user in allUsers" :key="user.id">
            {{user.name}}
            <h3 @click="deleteUser(user)">X</h3>
            </b-list-group-item>
  
        </b-list-group>
      </div>

      <!-- <ul>
        <li v-for="user in allUsers" :key="user.id">
          {{user.name}}
          <button @click="deleteUser(user)">Delete</button>
        </li>
      </ul>-->
    </b-container>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      user: {},
      name: ""
    };
  },
  async created() {
    let fetchUsers = await axios.get(
      "https://jsonplaceholder.typicode.com/users"
    );
    //  this.$store.commit("addUsers", user);
    this.$store.commit("setUsers", fetchUsers.data);
    console.log(this.$store.state.users);
  },
  computed: {
    allUsers() {
      return this.$store.state.users;
    }
  },
  methods: {
    update() {
      let user = {
        name: this.name
      };
      if (this.name == null) {
        alert("Invalid Username!!!");
      }
      this.name = "";
      this.$store.commit("addUsers", user);
    },
    deleteUser(user) {
      let users = this.$store.state.users.filter(item => {
        return item.name != user.name;
      });
      this.$store.commit("delUsers", users);
    }
  }
};
</script>
<style>
h3{
  float: right;
  color: red;
  font-size: 20px;
}
.user{
  width: 600px;
  margin-left: 400px;
  margin-top: 0px;
}
*{
  background-color: rgb(65, 61, 61);
  margin: 0;
}
</style>