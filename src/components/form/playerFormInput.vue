<template>
  <div>
    <v-ons-list-header>Player {{ num }}</v-ons-list-header>
    <v-ons-list-item>
      <div class="left">
        名　前
      </div>
      <div class="center">
        <v-ons-input
           placeholder="名前"
           v-model="name"
        ></v-ons-input>
      </div>
      <div class="right">
      </div>
    </v-ons-list-item>
    <v-ons-list-item>
      <div class="left">
        得　点
      </div>
      <div class="center">
        <v-ons-input
           type="number"
           placeholder="得点"
           v-model="score"
        ></v-ons-input>
      </div>
      <div class="right">
        {{ result }}
      </div>
    </v-ons-list-item>
    <v-ons-list-item>
      <div class="left">
        半荘数
      </div>
      <div class="center">
        <v-ons-select style="width: 40%"
          v-model="games"
        >
          <option v-for="n in 20" :value="n">
            {{ n }}
          </option>
        </v-ons-select>
      </div>
      <div class="right">
      </div>
    </v-ons-list-item>
    <v-ons-list-item>
      <div class="left">
        その他
      </div>
      <div class="center">
        <v-ons-input
           type="number"
           placeholder="例）カップ麺１個→ -250"
           v-model="option"
        ></v-ons-input>
      </div>
      <div class="right">
        円
      </div>
    </v-ons-list-item>
  </div>
</template>

<script>
  export default{
    name: 'PlayerFormInput',
    props: ['num', 'rate', 'playersInfo'],
    data () {
      return {
        name: '',
        score: '',
        games: '',
        option: '',
      }
    },
    computed: {
      winnings () {
        return this.score * this.rate * 10
      },
      result () {
        let sign = ''
        if (this.score > 0) {
          sign = '+'
        } else if (!this.score) {
          return '±0'
        }
        return sign + this.winnings
      }
    },
    mounted () {
      this.playersInfo[this.num] = {
        name: this.name,
        winnings: this.winnings,
        games: this.games,
        option: this.option,
      }
    },
    updated () {
      this.playersInfo[this.num] = {
        name: this.name,
        winnings: this.winnings,
        games: this.games,
        option: this.option,
      }
    },
    destroyed () {
      delete this.playersInfo[this.num]
    }
  }
</script>

<style scoped>
.list-item__center {
  margin-left: 0 16px
}
.list-item__center > ons-input {
  width: 100%;
}
</style>
