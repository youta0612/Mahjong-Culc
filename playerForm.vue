<template>
  <v-ons-page>
    <PlayerFormInput
      v-for="n in count"
      :key="n"
      :num="n"
    ></PlayerFormInput>
    <v-ons-button
      modifier="large"
      style="margin: 6px 0"
      @click="add"
    >追加</v-ons-button>
  </v-ons-page>
</template>

<script>
  import PlayerFormInput from './playerFormInput'
  export default{
    name: 'PlayerForm',
    components: {
      PlayerFormInput,
    },
    data () {
      return {
        count: 4,
      }
    },
    methods: {
      add () {
        this.count++
      },
    }
  };
</script>
