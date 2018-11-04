<template>
  <v-ons-page id="page-content" height="100%">
    <v-ons-navigator swipeable
      :page-stack="pageStack"
      @push-page="pageStack.push($event)"
    ></v-ons-navigator>
  </v-ons-page>
</template>

<script>
  import Form from './form/form'

  export default {
    name: 'Page',
    components: {
      Form,
    },
    data () {
      return {
        pageStack: [Form]
      }
    }
  }
</script>
