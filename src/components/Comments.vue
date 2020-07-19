<template>
  <div class="hello">
    <div id="todo-list-example" class="container">
      <div class="row">
        <div class="col-md-6 mx-auto">
          <h1 class="text-center">commentaires</h1>
          <form v-on:submit.prevent="addNewComment">
            <label for="commentInput">Votre commentaire</label>
            <input
              v-model="comment"
              id="commentInput"
              class="form-control"
              placeholder="ajouter un message"
            />
            <button
              v-if="this.isEdit == false"
              type="submit"
              class="btn btn-success btn-block mt-3"
            >Submit</button>
            <button
              v-else
              type="button"
              v-on:click="updateComment()"
              class="btn btn-primary btn-block mt-3"
            >Update</button>
          </form>

          <table class="table">
            <ul
              class="list-group"
              v-for="(comment) in comments"
              v-bind:key="comment.id"
              v-bind:title="comment.comment"
            >
              <li class="list-group-item">{{comment.id}}</li>
              <li class="list-group-item">{{comment.comment}}</li>
              <li class="list-group-item">
                <button
                  v-on:click="editComment(comment.title, comment.id)"
                  class="btn btn-info"
                >Edit</button>
                <button v-on:click="deleteComment(comment.id)" class="btn btn-danger">Supprimer</button>
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
    return {
      comments: [],
      id: "",
      comment: "",
      isEdit: false
    };
  },
  mounted() {
    this.getComments();
  },
  methods: {
    getComments() {
      this.comments = [];
      axios({ method: "GET", url: "http://localhost:5000/api/comments" }).then(
        result => {
          console.log(result.data);
          this.comments = result.data;
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

    addNewComment() {
      axios
        .post("http://localhost:5000/api/NewComment", {
          comment: this.comment,
          userId: 2
        })
        .then(res => {
          this.comment = "";
          this.getComments();
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

    editComment(title, id) {
      this.id = id;
      this.comment = title;
      this.isEdit = true;
    },

    updateComment() {
      axios
        .put(`http://localhost:5000/api/comment/${this.id}`, {
          comment: this.comment
        })
        .then(res => {
          this.comment = "";
          this.isEdit = false;
          this.getComments();
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    },

    deleteComment(id) {
      axios
        .delete(`http://localhost:5000/api/comment/${id}`)
        .then(res => {
          this.comment = "";
          this.getComments();
          console.log(res);
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>