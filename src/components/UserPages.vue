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
  }
}
</script>

<style scoped>

</style>
