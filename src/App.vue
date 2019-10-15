<template>
  <div id="app">
      <img src="../icon/48.png">
      <h1>Hello</h1>
      <input v-model="names">
      <p>{{ names }}</p>
      <h1>BudgetManager</h1>
      <h2>Total spend: <span>{{ spend }}</span></h2>
      <h2>Limit: <span>{{ limited }}</span></h2>
      <h3>Enter amount:</h3>
      <input type="text" v-model="amount">
      <input type="submit" @click="calc">
  </div>
</template>

<script>
  export default {
    
    name: 'app',

    mounted: function() {
      this.calc()
    },

    data () {
      return {
        names: '',
        spend: 0,
        limited: 0,
        amount: ''
      }
    },

    methods: {
      calc: function() {
        var vue = this;
        chrome.storage.sync.get(['total', 'limit'], function(budget){
          var newTotal = 0;
          vue.limited = budget.limit;
          vue.spend = budget.total;
          if(budget.total) {
            newTotal += parseInt(budget.total);
          }
          if(vue.amount) {
            newTotal += parseInt(vue.amount);
          }
          if(newTotal >= budget.limit || budget.total >= budget.limit) {
            var limitNotification = {
              type: 'basic',
              iconUrl: '../icon/48.png',
              title: 'Limit reached!',
              message: 'Uh oh! Looks like you\'ve reached your limit!'
            };
            chrome.notifications.create('limitNotif', limitNotification);
          }
          chrome.storage.sync.set({'total':newTotal});
          vue.spend = newTotal;
          vue.amount = '';
        });
      }
    }

  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 30px;
  }

  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
