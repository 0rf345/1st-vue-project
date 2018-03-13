<template>
  <div id="projectFooter">
    <p @click="roar()" class="projectFooter">{{ footerMsg }}</p>
  </div>
</template>

<script>
export default {
  name: 'ProjectFooter',
  data () {
    return {
      footerMsg: 'I am a footer, hear my roar'
    }
  },
  methods: {
    roar: function () {
      alert('Roar!!!')
    }
  }
}
</script>

<style>

.projectFooter {
  clear: both;
  padding: 0;
  text-align: center;
  vertical-align: middle;
  line-height: normal;
  margin: 0;
  float: bottom;
  bottom: 50px;
  position: relative;
  bottom: 0px;
  width: 100%;
}

.projectFooter:hover {
  cursor: pointer;
}
</style>
