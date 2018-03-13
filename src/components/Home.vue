<template>
  <div class="home">
    <!--
      Using v-ifs instead of v-show in order to use v-focus for being able to push Enter for +
      and autofocus on the new name input as soon as they are re-rendered.
      If these are not desirable for user use, we can just remove them and gain better performance instead.
    -->
    <div v-if="!selectedPage">
      <div v-if="!creatingSomething">
        <span>
          <button class="button">Settings</button>
          <button class="button" @click="createUserPagePage()" v-focus>+</button>
        </span>
        <div id="userPages">
          <p>User Created Pages</p>
          <p class="userPage" v-for="userPage in userPages" :key="userPage.id" @click="userPclicked(userPage)"> {{ userPage.name }}</p>
        </div>
      </div>
      <div v-show="creatingSomething">
        <userpages @UPsUpdated="updateUserPages($event)" @upcCancelled="cancelSubmission"></userpages>
      </div>
    </div>
    <div class="pageDetails" v-else>
      <pagedetails :page="clickedPage"></pagedetails>
    </div>
  </div>
</template>

<script>
import userpages from '@/components/UserPages'
import pagedetails from '@/components/PageDetails'

export default {
  name: 'Home',
  data () {
    return {
      creatingSomething: false,
      selectedPage: false,
      clickedPage: {},
      userPages: [],
      newPage: {}
    }
  },
  components: {
    userpages, pagedetails
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
      this.selectedPage = true
      this.clickedPage = userPage
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
