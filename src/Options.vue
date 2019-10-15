<template>
  <div id="root">
    <img src="../icon/48.png">
    <h1>BudgetManagerRoot</h1>
    <h2>Limit: <input v-model="amount" type="text" v-text="amount"></h2>
    <input type="submit" value="save" @click="saveButton">
    <input type="submit" value="reset" @click="resetButton">
  </div>
</template>

<script>
  export default {
    
    name: 'root',

    mounted: function() {
      chrome.storage.sync.get('limit', function(budget) {
        this.amount = budget.limit;
      });
      console.log(this.amount);
    },

    data () {
      return {
        amount: 0
      }
    },

    methods: {
      saveButton: function() {
        if(this.amount) {
          chrome.storage.sync.set({'limit': parseInt(this.amount)}, function() {
            close();
          });
        }
        console.log('save', this.amount);
      },

      resetButton: function() {
        chrome.storage.sync.set({'total': 0}, function() {
          var limitNotification = {
              type: 'basic',
              iconUrl: '../icon/48.png',
              title: 'Total reset!',
              message: 'Total has been reset to 0!'
            };
          chrome.notifications.create('limitNotif', limitNotification);
        });
        console.log('reset', this.amount);
      }
    }
    
  }
</script>
