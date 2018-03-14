<template>
  <div class="navbar">
    <ul>
      <li @click="$emit('homeSignal')"><a><router-link to="/">Home</router-link></a></li>
      <li><a><router-link to="mpla">Mpla</router-link></a></li>
      <li v-show="back" @click="$emit('backSignal')"><a><router-link to="/">Back</router-link></a></li>
      <!--
      <li v-for="page in pages" :key="page.id"><a @click="iWasClicked(page)">{{ page }}</a></li>
      <li class="dropdown">
        <a href="javascript:void(0)" class="dropbtn">Dropdown</a>
        <div class="dropdown-content">
          <a v-for="userP in userPages" :key="userP.id" href="#">{{ userP }}</a>
        </div>
      </li>
      -->
    </ul>
  </div>
</template>

<script>
export default {
  name: 'navbar',
  data () {
    return {
      pages: ['/Home', '/News', '/FakeNews'],
      userPages: ['ExPage1', 'ExPage2', 'ExPage3']
    }
  },
  props: [
    'back'
  ],
  methods: {
    iWasClicked: function (page) {
      console.log(page)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* Courtesy of w3schools.com */

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #333;
}

li {
  float: left;
}

li a, .dropbtn {
  display: inline-block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}

li a:hover, .dropdown:hover .dropbtn {
  background-color: red;
}

li.dropdown {
  display: inline-block;
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
  z-index: 1;
}

.dropdown-content a {
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  text-align: left;
}

.dropdown-content a:hover {background-color: #f1f1f1}

.dropdown:hover .dropdown-content {
  display: block;
}
</style>
