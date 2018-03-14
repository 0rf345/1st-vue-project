<template>
  <div class="userpages">
    <span>
      <form v-on:submit.prevent="createUserPage">
        <input type="text" v-model="newPage.name" placeholder="Name of new page" v-focus required />
        <button type="submit">Create</button>
        <button type="button" @click="cancelSubmission()">Cancel</button>
      </form>
    </span>
  </div>
</template>

<script>
export default {
  name: 'userpages',
  data () {
    return {
      userPages: [],
      newPage: {}
    }
  },
  props: [
    'impPages'
  ],
  methods: {
    createUserPage: function () {
      this.userPages.push(Object.assign({}, this.newPage))
      this.$emit('UPsUpdated', this.userPages)
      this.newPage.name = ''
    },
    cancelSubmission: function () {
      this.newPage.name = ''
      this.$emit('upcCancelled')
    }
  },
  directives: {
    focus: {
      // directive definition
      inserted: function (el) {
        el.focus()
      }
    }
  },
  watch: {
    impPages: function () {
      if (this.impPages !== []) {
        for (let page in this.impPages) {
          this.userPages.push(Object.assign({}, this.impPages[page]))
        }
        this.$emit('UPsUpdated', this.userPages)
      }
    }
  }
}
</script>

<style scoped>

</style>
