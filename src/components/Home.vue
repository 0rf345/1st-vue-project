<template>
  <div class="home">
    <!--
      Using v-ifs instead of v-show in order to use v-focus for being able to push Enter for +
      and autofocus on the new name input as soon as they are re-rendered.
      If these are not desirable for user use, we can just remove them and gain better performance instead.
    -->
    <div v-show="!selectedPage">
      <div v-if="!creatingSomething && !inSettings">
        <span>
          <button class="button" @click="inSettings=true">Settings</button>
          <button class="button" @click="createUserPagePage()" v-focus>+</button>
          <button class="button" @click="createJSONdata()">Download JSON Data</button>
          <label>Import JSON file: </label><input value="Choose JSON File" type="file" @change="importJSONdata" />
        </span>
        <div id="userPages">
          <p>User Created Pages</p>
          <p class="userPage" v-for="userPage in userPages" :key="userPage.id" @click="userPclicked(userPage)"> {{ userPage.name }}</p>
        </div>
      </div>
      <div v-show="inSettings">
        <p>Border color for user posts:</p>
        <br>
        <form @submit.prevent="postColorChange">
          <input type="color" required />
          <button type="submit">choose</button>
        </form>
      </div>
      <div v-show="creatingSomething">
        <userpages :impPages="importPages" @UPsUpdated="updateUserPages($event)" @upcCancelled="cancelSubmission"></userpages>
      </div>
    </div>
    <div class="pageDetails" v-show="selectedPage">
      <pagedetails :impP="shouldImport" :impPosts="importPosts" :stringAlert="iWantAstring" @JSONposts="getJSONposts" :borderColor="postBorderColor" :page="clickedPage" @changedNameOfUP="clickedPage.name = $event" @deletePage="deleteUserPage"></pagedetails>
    </div>
  </div>
</template>

<script>
import userpages from '@/components/UserPages'
import pagedetails from '@/components/PageDetails'
var FileSaver = require('file-saver')

export default {
  name: 'Home',
  data () {
    return {
      creatingSomething: false,
      selectedPage: false,
      inSettings: false,
      iWantAstring: false,
      shouldImport: false,
      clickedPage: {},
      userPages: [],
      newPage: {},
      importPages: [],
      importPosts: {},
      postBorderColor: '',
      jsonImportFile: ''
    }
  },
  components: {
    userpages, pagedetails
  },
  props: [
    'justBack', 'justHome'
  ],
  watch: {
    justBack: function () {
      if (this.justBack) {
        this.$emit('gotIt')
        this.creatingSomething = false
        this.selectedPage = false
      }
    },
    hustHome: function () {
      console.log('lala')
      if (this.justHome) {
        console.log('lala')
        this.$emit('gotIt')
        this.creatingSomething = false
        this.selectedPage = false
      }
    },
    jsonImportFile: function () {
      if (this.jsonImportFile !== '') {
        let jsonObj = JSON.parse(this.jsonImportFile)
        this.userPages = []
        for (let key in jsonObj.pages) {
          this.importPages.push({name: jsonObj.pages[key].name})
        }
        for (let key in jsonObj.posts) {
          this.importPosts[key] = Object.assign({}, jsonObj.posts[key])
        }
        this.shouldImport = true
      }
    }
  },
  methods: {
    createUserPagePage: function () {
      this.creatingSomething = true
    },
    updateUserPages: function (e) {
      this.userPages = e.slice()
      this.creatingSomething = false
    },
    cancelSubmission: function () {
      this.creatingSomething = false
    },
    userPclicked: function (userPage) {
      this.clickedPage = userPage
      this.selectedPage = true
      this.$emit('createBackButton')
    },
    deleteUserPage: function () {
      this.userPages.splice(this.userPages.indexOf(this.clickedPage), 1)
      this.clickedPage = {}
      this.creatingSomething = false
      this.selectedPage = false
    },
    postColorChange: function (e) {
      this.inSettings = false
      this.postBorderColor = e.target[0].value
    },
    createJSONdata: function () {
      this.iWantAstring = true
      console.log('Pages: ' + JSON.stringify(this.userPages))
    },
    getJSONposts: function (e) {
      this.iWantAstring = false
      var blob = new Blob([this.jsonPages + e], {type: 'text/plain;charset=utf-8'})
      FileSaver.saveAs(blob, 'project-data.json')
      console.log(this.jsonPages + e)
    },
    importJSONdata: function (e) {
      var files = e.target.files || e.dataTransfer.files
      if (!files.length) {
        return
      }
      var fr = new FileReader()

      var that = this
      fr.onload = function () {
        that.jsonImportFile = this.result
      }
      fr.readAsText(e.target.files[0])
    }
  },
  computed: {
    jsonPages: function () {
      return '{ "pages": ' + JSON.stringify(this.userPages) + ', '
    }
  },
  directives: {
    focus: {
      // directive definition
      inserted: function (el) {
        el.focus()
      }
    }
  }
}
</script>

<style scoped>
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

.userPage {
  margin: auto;
  margin-bottom: 5px;
  padding: 2px;
  border: 2px solid rgb(53, 125, 138);
  width: 50%;
  cursor: pointer;
}

.userPage:hover {
  background-color: greenyellow;
}

</style>
