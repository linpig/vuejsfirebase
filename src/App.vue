<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 & Firebase Sample Application</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Book</h3>
      </div>
      <div class="panel-body">
        <form class="" class="form-inline" v-on:submit.prevent="addBook">
            <div class="form-group">
              <label for="bookTitle">Title:</label>
              <input type="text" id="bookTitle" class="form-control" v-model="newBook.title" value="">
            </div>
            <div class="form-group">
              <label for="bookAuthor">Author:</label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author" value="">
            </div>
            <div class="form-group">
              <label for="bookUrl">URL:</label>
              <input type="text" id="bookUrl" class="form-control" v-model="newBook.url" value="">
            </div>
            <input type="submit" class="btn btn-primary" value="Add Book">
        </form>
      </div>
    </div>


    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books Lists</h3>
      </div>
    </div>
    <div class="panel-body">
      <table class="table table-striped">
        <thead>
           <tr>
             <th>
               Title
             </th>
             <th>
               Author
             </th>
             <th>
               Delete
             </th>

           </tr>
        </thead>
        <tbody>
          <tr v-for="book in books">
            <td>
              <a v-bind:href="book.url">{{book.title}}</a>
            </td>
            <td>
              {{book.author}}
            </td>
            <td>
              <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
            </td>

          </tr>
        </tbody>
      </table>
    </div>

  </div>
</template>

<script>

import Firebase from 'firebase'

import toastr from 'toastr'

let config = {
   apiKey: "AIzaSyDVZg6WbEElaAgIbnFo0jkIQlS8-0ndZYM",
   authDomain: "vuejs-dee09.firebaseapp.com",
   databaseURL: "https://vuejs-dee09.firebaseio.com",
   storageBucket: "vuejs-dee09.appspot.com",
   messagingSenderId: "879676610211"
}

let app = Firebase.initializeApp(config);
let db = app.database();

let booksRef = db.ref('books');



export default {
  name: 'app',
  firebase:{
    books:booksRef
  },
  data(){
    return {
      newBook: {
        title:'',
        author:'',
        url:''
      }
    }
  },
  methods: {
    addBook: function() {
      booksRef.push(this.newBook);
      this.newBook.title = '';
      this.newBook.author = '';
      this.newBook.url = '';
      toastr.success("新增一筆記錄囉");
    },
    removeBook: function(book){
      booksRef.child(book['.key']).remove();
      toastr.error("刪除一筆紀錄囉");
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /*text-align: center;*/
  color: #2c3e50;
  margin-top: 60px;
}
</style>
