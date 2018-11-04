<template>
  <v-ons-page>
    <Header
      :title="pageTitle"
    ></Header>
    <PlayerResult
      v-for="(info, index) in sortedResults"
      :key="index"
      :name="info.name || '名無し'"
      :winnings="info.winnings"
      :fee="oneGameFee * info.games"
      :option="info.option"
    ></PlayerResult>
  </v-ons-page>
</template>

<script>
  import Header from '../header'
  import PlayerResult from './playerResult'

  export default{
    name: 'Result',
    components: {
      Header,
      PlayerResult
    },
    props: ['amountFee', 'playersInfo'],
    data () {
      return {
        pageTitle: 'RESULT',
      }
    },
    computed: {
      allGames () {
        let total = 0
        for (let key in this.playersInfo) {
          total += Number(this.playersInfo[key].games)
        }
        return total / 4
      },
      oneGameFee () {
        let optionTotal = 0
        for (let key in this.playersInfo) {
          if (this.playersInfo[key].option) {
            optionTotal += Number(this.playersInfo[key].option)
          }
        }
        return (Number(this.amountFee) + optionTotal) / this.allGames / 4
      },
      sortedResults () {
        let results = []
        for (let key in this.playersInfo) {
          results.push(this.playersInfo[key])
        }
        results.sort((a, b) => {
          return b.winnings - a.winnings
        })
        return results
      }
    },
  }
</script>
