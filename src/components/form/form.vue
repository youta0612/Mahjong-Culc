<template>
  <v-ons-page>
    <Header
      :page-stack="pageStack"
      :title="pageTitle"
    ></Header>
    <v-ons-list>
      <v-ons-list-header>Rate</v-ons-list-header>
      <v-ons-list-item>
        <div class="left">
          風　速
        </div>
        <div class="center">
          <v-ons-select style="width: 40%"
            v-model="selectedRate"
          >
            <option v-for="rate in rateList" :value="rate">
              {{ rate }}
            </option>
          </v-ons-select>
        </div>
        <div class="right">
        </div>
      </v-ons-list-item>
      <v-ons-list-header>Charge</v-ons-list-header>
      <v-ons-list-item>
        <div class="left">
          <v-ons-icon icon="fa-yen-sign"></v-ons-icon>
          場　代
        </div>
        <div class="center">
          <v-ons-input
             type="number"
             placeholder="場代合計"
             v-model="amountFee"
          ></v-ons-input>
        </div>
        <div class="right">
          円
        </div>
      </v-ons-list-item>
      <PlayerFormInput
        v-for="n in count"
        :key="n"
        :num="n"
        :rate="selectedRate"
        :players-info="playersInfo"
      ></PlayerFormInput>
    </v-ons-list>
    <div class="btn-area" width="100%">
      <v-ons-button
        class="btn add-btn"
        modifier="outline"
        @click="add"
      >
        <v-ons-icon icon="fa-plus"></v-ons-icon>
        雀士追加
      </v-ons-button>
      <v-ons-button
        class="btn remove-btn"
        modifier="outline"
        @click="remove"
      >
        <v-ons-icon icon="fa-minus"></v-ons-icon>
        ひとり削除
      </v-ons-button>
      <v-ons-button
        class="btn next-btn"
        @click="next"
      >結果確認</v-ons-button>
    </div>
  </v-ons-page>
</template>

<script>
  import Header from '../header'
  import PlayerFormInput from './playerFormInput'
  import Result from '../result/result'

  export default {
    name: 'PlayerForm',
    components: {
      Header,
      PlayerFormInput,
    },
    props: ['pageStack'],
    data () {
      return {
        pageTitle: 'INPUT',
        count: 4,
        rateList: [0, 1, 3, 5, 10, 20],
        selectedRate: 5,
        amountFee: '',
        playersInfo: {},
      }
    },
    methods: {
      add () {
        this.count++
      },
      remove () {
        if (this.count > 4) {
          this.count--
        }
      },
      async next () {
        if (!this.validateAll()) {
          await this.$ons.notification.alert('入力項目に誤りがあります。', {
            callback () {
              return
            }
          })
        } else {
          this.$emit('push-page', {
            extends: Result,
            onsNavigatorProps: {
              amountFee: this.amountFee,
              playersInfo: this.playersInfo,
            }
          })
        }
      },
      checkSum () {
        let sum = 0
        for (let key in this.playersInfo) {
          sum += this.playersInfo[key].winnings || 0
        }
        return sum === 0
      },
      checkGames () {
        let sum = 0
        let max = 0
        for (let key in this.playersInfo) {
          if (!this.playersInfo[key].games) return false
          sum += Number(this.playersInfo[key].games)
          if (this.playersInfo[key].games) {
            max = Number(this.playersInfo[key].games)
          }
          console.log(sum, max)
        }
        return (sum % 4 === 0) && (max <= sum / 4)
      },
      validateAll () {
        console.log(this.checkSum(), this.checkGames())
        return (
          this.checkSum() &&
          this.checkGames()
        )
      }
    },
  }
</script>

<style scoped>
.list-item__center {
  margin-left: 0 16px
}
.btn {
  text-align: center;
  margin: 6px 1%;
}
.next-btn {
  width: 98%;
}
.add-btn,.remove-btn {
  border: 1px solid;
  background-color: none;
  width: 48%;
  float: left;
}
.remove-btn {
  color: tomato;
  border-color: tomato;
}
</style>
