<template>
  <div class="hello">
    <div id="todo-list-example" class="container">
      <div class="row">
        <div class="col-md-6 mx-auto">
          <h1 class="text-center">Liste des messages</h1>
          <form v-on:submit.prevent="addNewMessage">
            <label for="messageInput">Votre message</label>
            <input
              v-model="message"
              id="messageInput"
              class="form-control"
              placeholder="ajouter un message"
            />
            <div class="form-group">
              <label for="imageURL">Image URL</label>
              <input
                v-model="imageURL"
                type="url"
                class="form-control"
                id="imageURL"
                placeholder="Put an image URL"
              />
            </div>
            <button
              v-if="this.isEdit == false"
              type="submit"
              class="btn btn-success btn-block mt-3"
            >Submit</button>
            <button
              v-else
              type="button"
              v-on:click="updateMessage()"
              class="btn btn-primary btn-block mt-3"
            >Update</button>
          </form>

          <table class="table">
            <ul
              class="list-group"
              v-for="(message) in messages"
              v-bind:key="message.id"
              v-bind:title="message.message"
            >
              <li class="list-group-item">{{message.user.first_name }} {{ message.user.last_name}}</li>
              <li class="list-group-item">{{message.message}}</li>
              <img
                v-if="message.imageURL"
                class="mr-3"
                :src="message.imageURL"
                :alt="message.subject"
              />
              <li class="list-group-item">
                <button
                  v-on:click="editMessage(message.title, message.id)"
                  class="btn btn-info"
                >Edit</button>
                <button v-on:click="deleteMessage(message.id)" class="btn btn-danger">Supprimer</button>
                <button
                  v-on:click="seeComment(message.id)"
                  class="btn btn-primary"
                >Voir les commentaires</button>
              </li>
            </ul>

            <!-- <ul
              class="list-unstyled"
              v-for="(message) in messages"
              v-bind:key="message.id"
              v-bind:title="message.message"
            >
              <li class="text-left">{{message.message}}</li>
              <li class="text-left">
                <button
                  v-on:click="editMessage(message.title, message._id)"
                  class="btn btn-info"
                >Edit</button>
                <button v-on:click="deleteMessage(message.id)" class="btn btn-danger">Delete</button>
              </li>
            </ul>-->
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    const token = localStorage.usertoken;
    const decoded = jwtDecode(token);

    return {
      messages: [],
      id: "",
      message: "",
      isEdit: false,
      imageURL: "",
      userId: decoded.id,
      first_name: decoded.first_name,
      last_name: decoded.last_name,
      email: decoded.email
    };
  },
  mounted() {
    this.getMessages();
  },
  methods: {
    getMessages() {
      this.messages = [];
      axios({ method: "GET", url: "http://localhost:5000/api/messages" }).then(
        result => {
          console.log(result.data);
          this.messages = result.data;
        },
        error => {
          console.error(error);
        }
      );
    },
    // addNewTask() {
    //   axios
    //     .post("http:/api/task", { task_name: this.taskname })
    //     .then(res => {
    //       this.taskname = "";
    //       this.getTasks();
    //       console.log(res);
    //     })
    //     .catch(err => {
    //       console.log(err);
    //     });
    // },

    addNewMessage() {
      axios
        .post("http://localhost:5000/api/message", {
          imageURL: this.imageURL,
          message: this.message,
          userId: 2
        })
        .then(res => {
          this.message = "";
          this.messageURL;
          this.getMessages();
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    },

    // editTask(title, id) {
    //   this.id = id;
    //   this.taskname = title;
    //   this.isEdit = true;
    // },

    // updateTask(id) {
    //   axios
    //     .put(`http://localhost:5000/api/task/${id}`, {
    //       task_name: this.taskname
    //     })
    //     .then(res => {
    //       this.taskname = "";
    //       this.isEdit = false;
    //       this.getTasks();
    //       console.log(res);
    //     })
    //     .catch(err => {
    //       console.log(err);
    //     });
    // },

    editMessage(title, id) {
      this.id = id;
      this.message = title;
      this.isEdit = true;
    },

    updateMessage() {
      axios
        .put(`http://localhost:5000/api/message/${this.id}`, {
          message: this.message
        })
        .then(res => {
          this.message = "";
          this.isEdit = false;
          this.getMessages();
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    },

    deleteMessage(id) {
      axios.delete(`http://localhost:5000/api/message/${id}`),
        {
          userId: this.userId
        }
          .then(res => {
            this.message = "";
            this.getMessages();
            console.log(res);
          })
          .catch(err => {
            console.log(err);
          });
    }
  }
};
</script>
