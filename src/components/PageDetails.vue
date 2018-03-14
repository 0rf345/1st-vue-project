<template>
  <div class="pagedetails">
    <p>{{ pageName }}</p>
    <span>
      <button class="button" @click="edittingPage = true; addingPost = false">Edit Page</button>
      <button class="button" @click="addingPost = true; edittingPage = false">Add Post</button>
    </span>
    <div v-if="edittingPage">
      <form @submit.prevent="changeName">
        <input type="text" placeholder="Enter new name" required />
        <button type="submit">Change</button><button @click="edittingPage = false">Cancel</button>
      </form>
      <button class="red" @click="deletePage()">Delete Page</button>
    </div>
    <div v-if="addingPost">
      <form @submit.prevent="addPost">
        <input type="text" placeholder="Enter a post name" required />
        <button type="submit">Add this</button><button @click="addingPost = false">Cancel</button>
      </form>
    </div>
    <div id="userPosts">
      <p>Posts for this page</p>
      <p v-for="post in posts" :key="post.id" class="userPost">{{ post.name }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'pagedetails',
  data () {
    return {
      posts: [],
      edittingPage: false,
      addingPost: false,
      pageName: this.page.name
    }
  },
  props: [
    'page'
  ],
  computed: {
    currentPage: function () {
      return this.page.slice()
    }
  },
  methods: {
    changeName: function (e) {
      console.log(e)
      //  Value of input field
      this.pageName = e.target[0].value
      this.edittingPage = false
      this.$emit('changedNameOfUP', this.pageName)
    },
    addPost: function (e) {
      this.posts.push({name: e.target[0].value})
      this.addingPost = false
    },
    deletePage: function () {
      this.$emit('deletePage')
    }
  }
}
</script>

<style>
.button {
  background-color: #4CAF50; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  -webkit-transition-duration: 0.4s; /* Safari */
  transition-duration: 0.4s;
}

.button:hover {
  box-shadow: 0 12px 16px 0 rgba(0,0,0,0.24),0 17px 50px 0 rgba(71, 226, 231, 0.19);
}

.userPost {
  margin: auto;
  margin-bottom: 5px;
  padding: 2px;
  border: 2px solid rgb(134, 52, 189);
  width: 50%;
  cursor: pointer;
}

.userPost:hover {
  background-color: rgb(203, 245, 255);
}

.red {
  background-color: red;
}
</style>
