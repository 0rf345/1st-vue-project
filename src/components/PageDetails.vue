<template>
  <div class="pagedetails">
    <p><b><h1>{{ page.name }}</h1></b></p>
    <span v-show="!edittingPage && !addingPost && !focOnPost">
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
    <div id="userPosts" v-show="!focOnPost">
      <p>Posts for this page</p>
      <p v-for="post in posts[pageName]" :key="post.id" class="userPost" :style="{ border: myBorder }" @click="userPostClicked(post)">{{ post.name }}</p>
    </div>
    <div v-show="focOnPost">
      <p>{{ focPost.name }}</p>
      <br>
      <form @submit.prevent="changePostName">
        <input type="text" placeholder="Type new name" required />
        <button type="submit">Change</button>
      </form>
      <button class="red" @click="deletePost()">Delete Post</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'pagedetails',
  data () {
    return {
      posts: {},
      edittingPage: false,
      addingPost: false,
      focOnPost: false,
      focPost: {}
    }
  },
  props: [
    'page', 'borderColor', 'stringAlert', 'impPosts', 'impP'
  ],
  methods: {
    changeName: function (e) {
      //  Value of input field
      //  this.pageName = e.target[0].value
      this.edittingPage = false
      this.posts[e.target[0].value] = this.posts[this.page.name]
      delete this.posts[this.page.name]
      this.$emit('changedNameOfUP', e.target[0].value)
    },
    addPost: function (e) {
      if (this.posts[this.pageName] === undefined) {
        this.posts[this.pageName] = []
      }
      this.posts[this.pageName].push({name: e.target[0].value})
      this.addingPost = false
    },
    deletePage: function () {
      this.$emit('deletePage')
    },
    userPostClicked: function (post) {
      this.focOnPost = true
      this.focPost = post
    },
    changePostName: function (e) {
      this.focPost.name = e.target[0].value
      this.focOnPost = false
    },
    deletePost: function () {
      this.focOnPost = false
      this.posts[this.pageName].splice(this.posts[this.pageName].indexOf(this.focPost), 1)
    }
  },
  computed: {
    myBorder: function () {
      return '2px solid ' + this.borderColor
    },
    pageName: function () {
      return this.page.name
    },
    currentPosts: function () {
      return this.posts[this.page.name]
    }
  },
  watch: {
    stringAlert: function () {
      if (this.stringAlert) {
        this.$emit('JSONposts', '"posts": ' + JSON.stringify(this.posts) + ' }')
      }
    },
    impP: function () {
      for (let key in this.impPosts) {
        this.posts[key] = []
        for (let keykey in this.impPosts[key]) {
          this.posts[key].push({ name: this.impPosts[key][keykey].name })
        }
      }
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
