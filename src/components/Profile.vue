<template>
  <div class="container">
    <div class="jumbotron mt-5">
      <div class="col-sm-8 mx-auto">
        <h1 class="text-center">PROFILE</h1>
      </div>
      <table class="table col-md-6 mx-auto">
        <tbody>
          <tr>
            <td>Fist Name</td>
            <td>{{first_name}}</td>
          </tr>
          <tr>
            <td>Last Name</td>
            <td>{{last_name}}</td>
          </tr>
          <tr>
            <td>Email</td>
            <td>{{email}}</td>
            <td>{{id}}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <button v-on:click="deleteUser(id)" class="btn btn-danger">Supprimer Profil</button>
  </div>
</template>

<script>
import jwtDecode from "jwt-decode";
import axios from "axios";

export default {
  data() {
    const token = localStorage.usertoken;
    const decoded = jwtDecode(token);
    return {
      id: decoded.id,
      first_name: decoded.first_name,
      last_name: decoded.last_name,
      email: decoded.email
    };
  },

  methods: {
    deleteUser(id) {
      axios
        .delete(`http://localhost:5000/users/profile/${id}`)
        .then(res => {
          "profile supprimé";
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>
