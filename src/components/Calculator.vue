<template>
  <b-container>
    <b-row>
      <b-col cols="12">
        <p>
          <label>每月花費 (USD):</label>
          <input type="text" v-model="spend" id="spend" @keyup.enter="calculatorSaved" />
          <label>MCO價格 (USD):</label>
          <input type="text" v-model="mcoPrice" id="mcoPrice" @keyup.enter="calculatorSaved" />
        </p>
        <b-input-group prepend=0 append="10000" class="mt-3">
          <b-form-input type="range" min=0 max="10000" v-model="spend" @change="calculatorSaved"></b-form-input>
        </b-input-group>
        <b-button href="#" variant="primary" @click="calculatorSaved">Calculator</b-button>
      </b-col>

      <b-col cols="12">
        <div>
          <b-card header="年回饋" header-tag="header">
            <b-table striped hover :items="items" :fields="fields"></b-table>
          </b-card>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "Calculator",
  data() {
    return {
      mcoPrice: 4,
      spend: 0,
      midnight_blue: {
        card_type: "midnight_blue",
        rewards: 0.01,
        interest_rate: 0,
        stake: 0,
        total_saved: 0,
        cashback: 0,
        interest: 0,
        spotify: 0,
        netflix: 0,
        amazon_prime: 0,
        break_even: 0
      },
      ruby_steel: {
        card_type: "ruby_steel",
        rewards: 0.02,
        interest_rate: 0,
        stake: 50,
        total_saved: 0,
        cashback: 0,
        interest: 0,
        spotify: 155.88,
        netflix: 0,
        amazon_prime: 0,
        break_even: 0
      },
      royal_indigo: {
        card_type: "royal_indigo",
        rewards: 0.03,
        interest_rate: 0.06,
        stake: 500,
        total_saved: 0,
        cashback: 0,
        interest: 30,
        spotify: 155.88,
        netflix: 155.88,
        amazon_prime: 0,
        break_even: 0
      },
      icy_white: {
        card_type: "icy_white",
        rewards: 0.04,
        interest_rate: 0.08,
        stake: 5000,
        total_saved: 0,
        cashback: 0,
        interest: 400,
        spotify: 155.88,
        netflix: 155.88,
        amazon_prime: 155.88,
        break_even: 0
      },
      obsidian_black: {
        card_type: "obsidian_black",
        rewards: 0.05,
        interest_rate: 0.08,
        stake: 50000,
        total_saved: 0,
        cashback: 0,
        interest: 4000,
        spotify: 155.88,
        netflix: 155.88,
        amazon_prime: 155.88,
        break_even: 0
      },
      fields: [
        { key: "card_type", label: "卡種" },
        { key: "stake", label: "鎖倉" },
        { key: "total_saved", label: "總回饋" },
        { key: "cashback", label: "刷卡回饋" },
        { key: "interest", label: "鎖倉利息" },
        { key: "spotify", label: "Spotify" },
        { key: "netflix", label: "Netflix" },
        { key: "amazon_prime", label: "Amazon Prime" },
        { key: "break_even", label: "回本時間" }
      ],
      items: [
        {
          card_type: "午夜藍",
          stake: "MCO 0",
          total_saved: 0,
          cashback: 0,
          interest: 0,
          spotify: 0,
          netflix: 0,
          amazon_prime: 0,
          break_even: 0
        },
        {
          card_type: "寶石紅",
          stake: "MCO 50",
          total_saved: 0,
          cashback: 0,
          interest: 0,
          spotify: "$155.88",
          netflix: 0,
          amazon_prime: 0,
          break_even: 0
        },
        {
          card_type: "皇家靛藍",
          stake: "MCO 500",
          total_saved: 0,
          cashback: 0,
          interest: 30,
          spotify: "$155.88",
          netflix: "$155.88",
          amazon_prime: 0,
          break_even: 0
        },
        {
          card_type: "冰冷白",
          stake: "MCO 5000",
          total_saved: 0,
          cashback: 0,
          interest: 400,
          spotify: "$155.88",
          netflix: "$155.88",
          amazon_prime: "$155.88",
          break_even: 0
        },
        {
          card_type: "黑曜黑",
          stake: "MCO 50000",
          total_saved: 0,
          cashback: 0,
          interest: 4000,
          spotify: "$155.88",
          netflix: "$155.88",
          amazon_prime: "$155.88",
          break_even: 0
        }
      ]
    };
  },
  mounted() {

  },
  methods: {
    calculatorSaved() {
      let that = this;
      
      // Total saved
      this.totalSaved(that.midnight_blue);
      this.totalSaved(that.ruby_steel);
      this.totalSaved(that.royal_indigo);
      this.totalSaved(that.icy_white);
      this.totalSaved(that.obsidian_black);

      this.updateTable(0, that.midnight_blue);
      this.updateTable(1, that.ruby_steel);
      this.updateTable(2, that.royal_indigo);
      this.updateTable(3, that.icy_white);
      this.updateTable(4, that.obsidian_black);
    },
    updateTable(item, card_type) {
      let that = this;
      that.items[item].stake = "MCO "+card_type.stake +" ($"+ card_type.stake*that.mcoPrice +")";
      that.items[item].total_saved = "$"+card_type.total_saved;
      that.items[item].cashback = "$"+card_type.cashback;
      that.items[item].interest = "$"+card_type.interest;
      that.items[item].break_even = card_type.break_even+" 天";
    },
    totalSaved(card_type){
        let that = this;
        card_type.cashback = Number(card_type.rewards * that.spend * 12).toFixed(2);
        card_type.interest = Number(card_type.stake * card_type.interest_rate * that.mcoPrice).toFixed(2);
        card_type.total_saved = Number(Number(card_type.cashback) + Number(card_type.interest) + Number(card_type.spotify) + Number(card_type.netflix) + Number(card_type.amazon_prime)).toFixed(2);
        let temp_stake = card_type.stake*that.mcoPrice;
        card_type.break_even = Math.ceil(temp_stake / (card_type.total_saved/365));
    }
  }
};
</script>