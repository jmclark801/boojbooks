<template>
  <div class="books">
    <div class="holder">
      <form v-on:submit.prevent="searchForBook(book)" autocomplete="off">
        <input
          type="text"
          placeholder="Please enter a book to search for "
          v-model="book"
          v-validate="'min:3'"
          name="search"
        >
        <transition
          name="alert-in"
          enter-active-class="animated flipInX"
          leave-active-class="animated flipOutX"
        >
          <p class="alert" v-if="errors.has('search')">{{ errors.first('search') }}</p>
        </transition>
      </form>
      <ul>
        <li v-for="result in results">
          <!-- This warning wants a key when it is not required per vue docs-->
          <img v-bind:src="result.thumbnail">
          <p>
            {{ result.author }}
            <br>
            {{ result.title }}
            <br>
            {{ result.averageRating }}
            <br>
          </p>
          <i class="fa fa-plus-circle" v-on:click="remove(index)"></i>
        </li>
      </ul>
      <ul>
        <transition-group
          name="list"
          enter-active-class="animated bounceInUp"
          leave-active-class="animated bounceOutDown"
        >
          <li v-for="(data, index) in books" :key="index">
            {{ data.book }}
            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
          </li>
        </transition-group>
      </ul>
      <!-- <p>Your Favorite Books!</p> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Books",
  data() {
    return {
      book: "",
      books: [],
      results: []
    };
  },
  methods: {
    searchForBook(book) {
      this.$validator.validateAll().then(result => {
        if (result) {
          console.log(`Seaching for ${book}`);
          axios
            .get(`https://www.googleapis.com/books/v1/volumes?q=` + book)
            .then(response => {
              for (var i = 0; i < 3; i++) {
                this.results.push({
                  author: response.data.items[i].volumeInfo.authors[0],
                  thumbnail:
                    response.data.items[i].volumeInfo.imageLinks.smallThumbnail,
                  title: response.data.items[i].volumeInfo.title,
                  averageRating: response.data.items[i].volumeInfo.averageRating
                });
                console.log(`Author: ${this.results[i].author}`);
              }
              console.log(this.results);
            });

          this.books.push({ book: this.book });
          this.book = "";
        } else {
          console.log("Not a valid search");
        }
      });
    },
    remove(id) {
      this.books.splice(id, 1);
    }
  }
};
</script>

<style scoped>
@import "https://cdn.jsdelivr.net/npm/animate.css@3.5.1";
@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

.alert {
  background: #fdf2ce;
  font-weight: bold;
  display: inline-block;
  padding: 5px;
  margin-top: -20px;
}

.holder {
  background: #fff;
}

i {
  float: right;
}

input {
  width: calc(100% - 40px);
  border: 0;
  padding: 20px;
  font-size: 1.3em;
  background-color: #323333;
  color: #687f7f;
}

ul {
  margin: 0;
  padding: 0;
  list-style-type: none;
}

ul li {
  padding: 20px;
  font-size: 1.3em;
  background-color: #e0edf4;
  border-left: 3px solid #3eb3f6;
  margin-bottom: 2px;
  color: #3e5252;
}

p {
  text-align: center;
  padding: 30px 0;
  color: gray;
}

.container {
  box-shadow: 0px 0px 40px lightgray;
}
</style>
