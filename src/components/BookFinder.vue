<template>
  <div class="hello">
    <h1>Book Finder Application</h1>
    <form @submit.prevent="onSubmit">
      <p>
        <input type="search" placeholder="Search Books" v-model="search" />
      </p>
      <button class="btn">Submit</button>
    </form>
    <div v-for="(book, index) in books" :key="index">
      <p>Title : {{book.volumeInfo.title}} </p>
      <label>Authors:<p v-for="(author, index) in book.volumeInfo.authors" :key="index">{{author}}</p></label>
      <img :src="book.volumeInfo.imageLinks.smallThumbnail"><br>
      <a :href="book.volumeInfo.previewLink" target="_blank">Preview Book</a>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "BookFinder",
  data() {
    return {
      books: {},
      search: ""
    };
  },
  methods: {
    onSubmit() {
      if (this.search === "") {
        alert("Please enter something in the search field");
      } else {
        axios
          .get("https://www.googleapis.com/books/v1/volumes?q=" + this.search)
          .then(response => {
            this.books = response.data.items;
          });
      }
      this.search = "";
      this.books = {};
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
input {
  width: 300px;
  height: 150px;
}
.btn {
  background-color: red;
  cursor: pointer;
}
</style>
