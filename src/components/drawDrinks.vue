  <Layout>
        <line-chart></line-chart>   
  </Layout>

<script>
import { Line } from 'vue-chartjs'


var chartcolors = ['#9A6E69','#FBEDE5','#C9C0AF','#6A7C8B','#014830']


export default {
    extends: Line,
  
mounted () {
      this.renderLineChart()
  },
watch: {
    data: function() {
      this._chart.update();
      //this.renderLineChart();
    }
  },
  data () {
    return {
      series: [],
      hours: [0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23]
      }
  },
  computed: {
      caffeinLevel: function() {
        return this.wrapCoffee();
      },
      getCurrentHour: function(){
        var currentdate = new Date(); 
        return currentdate.getHours() 
      }
    },
  methods: {
    addSeries: function (name_input, hour_input) {
      this.series.push({name: name_input, hour:hour_input})
      this.renderLineChart()
    },
    wrapCoffee: function () {
        var out = [];
        var item_data = [];
        if (this.series && this.series.length) {
          this.series.forEach((item, index) => {
           

          if (item['name'] == 'coffee') {
            item_data = this.hours.map(x => this.makeCoffee(x, item['hour']))
          }

          if (item['name'] == 'tea') {
            item_data = this.hours.map(x => this.makeTea(x, item['hour']))
          }

            var drink = {
                label: item['name'] + ' at ' + item['hour'], 
                data: item_data,
                backgroundColor: chartcolors[index],
                            pointRadius:0,

              }
            out.push(drink)
          })
        }
      return out;
    },
    makeCoffee: function (x, loc) {
      var k = 1.1;
      var l = 1;
      return ((k/l)*((x-loc)/l)**(k-1))*Math.pow(2.7182, (-(x-loc)/l))**k || 0;  
    },
   makeTea: function (x, loc) {
      var k = 1.1;
      var l = 1.2;
      return ((k/l)*((x-loc)/l)**(k-1))*Math.pow(2.7182, (-(x-loc)/l))**k || 0;  
    },
    renderLineChart: function() {
        this.renderChart({
          labels: this.hours,
          datasets: this.caffeinLevel
          }, 
          {
            responsive: true, 
            animation: {easing: "easeInOutBack"},
            maintainAspectRatio: false,
            scales:{
              yAxes: [{stacked: true,
                gridLines: {
                  display:false
            }}],
              xAxes: [{ticks: {
                min: this.getCurrentHour-2,
                max: this.getCurrentHour +6
              },
                gridLines: {
                  display:false
            }},
              ]
              }
          }
        )
    }
  }
}

</script>
