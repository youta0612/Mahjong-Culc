<template>
  <v-ons-toolbar>
    <div class="left">
      <v-ons-back-button>Back</v-ons-back-button>
    </div>
    <div class="center">{{ title }}</div>
  </v-ons-toolbar>
</template>

<script>
  export default{
    name: 'Header',
    props: ['pageStack', 'title'],
  };
</script>
