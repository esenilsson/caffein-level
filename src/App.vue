<template>
  <div class='lay'>
    <div class='mainCenter'>
      <h1>The time is {{getCurrentHour}}-  How is your caffein situation?</h1>
      <drinkChart ref="chart" id='chart'></drinkChart>
      <!-- <button v-on:click='addDrink'>Add drink</button> -->
      <div >
      <form class='add-coffee' @submit.prevent="addDrink">
        <input type="radio" id="coffee" name="drinktype" value="coffee" checked=true>
        <label class='form-labels' for="coffee">Coffee</label>
        <input type="radio" id="tea" name="drinktype" value="tea">
        <label class='form-labels' for="tea">Tea</label>
        <input type="text" id="coffeetime" name="coffeetime" :value=this.getCurrentHour>
        <input type="submit" value="add">
      </form>
      </div>

    <!-- <table>
      <tr>
        <th>Drink type</th>
        <th>Hour</th>
      </tr>
      <template v-for="(cnt, idx) in drinks.length">
        <tr>
        <td>{{ drinks[idx].drinktype}}</td>
        <td>{{ drinks[idx].drinktime}}</td>
        </tr>
      </template>
  </table> -->
    

    </div>
    </div>
</template>

<script>

import drinkChart from './components/drawDrinks.vue';

export default {
  components: {
        'drinkChart': drinkChart
    },
  data () {
    return {y: 0,
            drinks: []
            }
  },
  metaInfo: {
    title: 'CoffeeTracker'
  },
  methods: {
    addDrink(formData) {
      console.log(formData.target.drinktype.value)
      this.$refs.chart.addSeries(formData.target.drinktype.value, 
                  formData.target.coffeetime.value)
      this.drinks.push({'drinktype': formData.target.drinktype.value, 
                  'drinktime': formData.target.coffeetime.value})
    }
  },
  computed: {
    getCurrentHour: function(){
        var currentdate = new Date(); 
        return currentdate.getHours() 
      }
  }
}

</script>

<style>
.home-links a {
  margin-right: 1rem;
}
</style>


// data:{
// 		datasets: [{
// 			label:"Series 1",
// 			data:s1,
// 			borderColor:"red",
// 			backgroundColor:"transparent",
// 		},
// 		{
// 			label:"Series 2",
// 			data:s2,
// 			borderColor:"blue",
// 			backgroundColor:"transparent",
// 		}],
// 	},