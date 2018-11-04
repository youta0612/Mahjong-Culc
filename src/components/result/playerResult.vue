<template>
  <v-ons-card>
    <div class="title" style="font-size: xx-large">
      {{ name }}<span :class="{ plus: isPlus(total), minus: isMinus(total) }">（{{ Math.round(total) }} 円）</span>
    </div>
    <div class="gif-area" style="text-align: center;">
      <img :src="selectGif" alt="" style="width: 60%">
    </div>
    <v-ons-list>
      <v-ons-list-header>Breakdown</v-ons-list-header>
      <v-ons-list-item>
        <div class="left"> {{ winningsLabel }} </div>
        <div class="right">
          <span :class="{ plus: isPlus(winnings), minus: isMinus(winnings) }">{{ winnings }} 円</span>
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left"> 場代 </div>
        <div class="right">
          <span :class="{ plus: isPlus(-fee), minus: isMinus(-fee) }">{{ -Math.round(fee) }} 円</span>
        </div>
      </v-ons-list-item>
      <v-ons-list-item>
        <div class="left"> その他 </div>
        <div class="right">
          <span :class="{ plus: isPlus(option), minus: isMinus(option) }">{{ option || 0 }} 円</span>
        </div>
      </v-ons-list-item>
    </v-ons-list>
  </v-ons-card>
</template>

<script>
  export default{
    name: 'PlayerResult',
    props: ['name', 'winnings', 'fee', 'option'],
    computed: {
      winningsLabel () {
        if (this.winnings >= 0) {
          return '勝ち分'
        } else {
          return '負け分'
        }
      },
      total () {
        return Number(this.winnings) - Number(this.fee) + Number(this.option)
      },
      selectGif () {
        const dir = this.total > 0 ? 'winner' : 'loser'
        return require(`../../assets/gif_${dir}/${this.getRandomInt(20)}.gif`)
      },
    },
    methods: {
      getRandomInt (max) {
        return ('00' + Math.floor(Math.random() * Math.floor(max))).slice(-2);
      },
      isPlus (val) {
        return val > 0
      },
      isMinus (val) {
        return val < 0
      }
    },
  }
</script>

<style scoped>
.gif-area {
  text-align: center;
}
.plus {
  color: #0076ff;
}
.minus {
  color: tomato;
}
</style>
