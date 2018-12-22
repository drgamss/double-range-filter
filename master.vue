<template>
    <div class='container'>
      <div class="row">
        <div class="col-md-8 col-md-offset-2">
        <section class="range-slider">
          Filter cars by price: {{ lowerRange + '$' }} - {{ higherRange + '$'}}
          <input @change="filterCars"  class='range-input' :min="min" :max="max" step="500" type="range" v-model='firstRange'>
          <input @change="filterCars" class='range-input' :min="min" :max="max" step="500" type="range" v-model='secondRange'>
        </section>
        <table class="table">
            <thead>
              <tr>
                <th>Car Model</th>
                <th>Car Manufacturer</th>
                <th>Year</th>
                <th>Price</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(car, index) in filteredCars" :key="index">
                <td>{{ car.model }}</td>
                <td>{{ car.manufacturer }}</td>
                <td>{{ car.year }}</td>
                <td>{{ car.price }}</td>
              </tr>
            </tbody>
          </table> 
          </div>  
        </div>     
    </div>
</template>
<script>
import data from './data.js'; 
export default {
    created: function(){
      this.cars = data.data;
      this.filteredCars = this.cars.sort(this.compare);
    },
    data: function(){
      return {
        cars: [],
        filteredCars: [],
        min: 5000,
        max: 100000,
        firstRange: '5000',
        secondRange: '100000'
      }
    },
    methods: {
      filterCars: function(){
        var that = this;
        var filteredCars = this.cars.filter(function(item){
          return item.price > that.lowerRange && item.price < that.higherRange
        })
        this.filteredCars = filteredCars.sort(this.compare);
      },
      compare: function(a, b){
        let comparison = 0;
        if (a.model > b.model) {
          comparison = 1;
        } else if (b.model > a.model) {
          comparison = -1;
        }
        return comparison;
      }
    },
    computed: {
      lowerRange: function(){
        return parseInt(this.firstRange) > parseInt(this.secondRange) ? this.secondRange : this.firstRange
      },
      higherRange: function(){
        return parseInt(this.firstRange) < parseInt(this.secondRange) ? this.secondRange : this.firstRange
      }
    }
}
</script>

<style lang='scss'>
@mixin range-slider($width, $height, $input-top, $input-bg-color, $input-thumb-color, $float:none, $input-height:20px, $input-border-radius:14px) {
  position: relative;
  width: 300px;
  height: 110px;
  margin: 0 auto;
  padding-top: 15px;
  text-align: center;
 
  input[type="range"] {
    pointer-events: none;
    cursor: pointer;
    position: absolute;
    -webkit-appearance: none;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);    
    border: none;
    border-radius: $input-border-radius;
    background: $input-bg-color;
    box-shadow: inset 0 1px 0 0 darken($input-bg-color,15%), inset 0 -1px 0 0 darken($input-bg-color,10%);
    -webkit-box-shadow: inset 0 1px 0 0 darken($input-bg-color,15%), inset 0 -1px 0 0 darken($input-bg-color,10%);
    overflow: hidden;
    left: 0;
    top: $input-top;
    width: $width;
    outline: none;
    height: $input-height;
    margin: 0;
    padding: 0;
  }  
  input[type="range"]::-webkit-slider-thumb {
    pointer-events: all;
    position: relative;
    z-index: 1;
    outline: 0;    
    -webkit-appearance: none;
    width: $input-height;
    height: $input-height;
    border: none;
    border-radius: $input-border-radius;
    background-image: -webkit-gradient(linear, left top, left bottom, color-stop(0%, lighten($input-thumb-color,60%)), color-stop(100%, $input-thumb-color)); /* android <= 2.2 */
    background-image: -webkit-linear-gradient(top , lighten($input-thumb-color,60%) 0, $input-thumb-color 100%); /* older mobile safari and android > 2.2 */;
    background-image: linear-gradient(to bottom, lighten($input-thumb-color,60%) 0, $input-thumb-color 100%); /* W3C */
  }
  input[type="range"]::-moz-range-thumb {
    pointer-events: all;
    position: relative;
    z-index: 10;
    -moz-appearance: none;
    width: $input-height;
    height: $input-height;
    border: none;
    border-radius: $input-border-radius;
    background-image: linear-gradient(to bottom, lighten($input-thumb-color,60%) 0, $input-thumb-color 100%); /* W3C */
  }
  input[type="range"]::-ms-thumb {
    pointer-events: all;
    position: relative;
    z-index: 10;
    -ms-appearance: none;
    width: $input-height;
    height: $input-height;
    border-radius: $input-border-radius;
    border: 0;
    background-image: linear-gradient(to bottom, lighten($input-thumb-color,60%) 0, $input-thumb-color 100%); /* W3C */
  }
  input[type=range]::-moz-range-track {
    position: relative;
    z-index: -1;
    background-color: rgba(0, 0, 0, 1);
    border: 0;
  }
  input[type=range]:last-of-type::-moz-range-track {
    -moz-appearance: none;
    background: none transparent;
    border: 0;
  } 
  input[type=range]::-moz-focus-outer {
    border: 0;
  }
}
.container {
  margin-top: 30px;
  section.range-slider {
    @include range-slider(300px, 110px, 50px, #F1EFEF, #413F41);
  }  
}


@import "../../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap/variables";
@import "../../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap/mixins";
@import "../../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap/normalize";
@import "../../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap/scaffolding";
@import "../../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap/grid";
@import "../../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap/tables";
@import "../../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap/utilities";
@import "../../../../../node_modules/bootstrap-sass/assets/stylesheets/bootstrap/responsive-utilities";
</style>
