<template>
  <div class="container">
    <h3>All Users</h3>
    <div v-if="message" class="alert alert-success">
      {{ this.message }}
    </div>
    <div v-if="this.aparece">{{ this.mensagem1 }}</div>
    <div class="column is-12">
      <button class="btn btn-warning" v-on:click="apareceAlgo()">click</button>
    </div>
    <div class="container">
      <table class="table">
        <thead>
          <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Email</th>
            <th>Update</th>
            <th>Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users" v-bind:key="user.id">
            <td>{{ user.firstName }}</td>
            <td>{{ user.lastName }}</td>
            <td>{{ user.email }}</td>
            <td>
              <button class="btn btn-warning" v-on:click="updateUser(user.id)">
                Update
              </button>
            </td>
            <td>
              <button class="btn btn-danger" v-on:click="deleteUser(user.id)">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
      <div class="row">
        <button class="btn btn-success" v-on:click="addUser()">Add</button>
      </div>
    </div>
  </div>
</template>
<script>
import UserDataService from '../service/UserDataService';

export default {
  name: 'Users',
  data() {
    return {
      users: [],
      message: '',
      mensagem1: 'Apareceu mensagem...',
      aparece: false
    };
  },
  methods: {
    apareceAlgo() {
      this.aparece = !this.aparece;
    },

    refreshUsers() {
      UserDataService.retrieveAllUsers().then((res) => {
        this.users = res.data;
      });
    },
    addUser() {
      this.$router.push(`/user/-1`);
    },
    updateUser(id) {
      this.$router.push(`/user/${id}`);
    },
    deleteUser(id) {
      UserDataService.deleteUser(id).then(() => {
        this.refreshUsers();
      });
    }
  },
  created() {
    this.refreshUsers();
  }
};
</script>