<template>
  <div class="container">
    <Header></Header>
    <div class="body">
        <Card v-if="weatherData&&nowweatherData&&currentweatherData":currentweatherData="currentweatherData" :weather-data="weatherData" :nowweatherData="nowweatherData"></Card>
        <Chart v-if="weatherData&&nowweatherData&&currentweatherData":currentweatherData="currentweatherData" :weather-data="weatherData" :nowweatherData="nowweatherData" :nextWeek="nextweek"></Chart>

    </div>
  </div>
</template>

<script>
import Header from './components/Header/index.vue';
import Card from './components/Card/index.vue';
import Chart from './components/Chart/index.vue';
import axios from 'axios';
export default{
components:{
  Header,
  Card,
  Chart
},
data(){
        return{
            weatherData:'',
            nowweatherData:'',
            currentweatherData:'',
            nextweek:[],
        }
    },
    mounted(){
        this.getWeather()
    },
    methods:{
        getWeather(){
           const response = axios.get('http://v1.yiketianqi.com/api?unescape=1&version=v9&appid=84686773&appsecret=JunYchW8')
           .then(response => {  
                this.weatherData = response.data || {};
                this.nowweatherData = this.weatherData.data?.[0] || {}
                this.currentweatherData = this.nowweatherData.hours[0] || {};
                for(let i=0;i<7;i++){
                    this.nextweek.push(this.weatherData.data[i])
                }
               
                console.log(this.nextweek) 
            })
            .catch(error => {  
                console.error('请求天气数据时出错:', error);  
            }); 
        },
    }
}
</script>

<style>
html,p,body{
    margin: 0;
    padding: 0;
}
.container{
    width: 100%;
    min-height: 600px;
    background: linear-gradient(rgb(72, 86, 99), rgb(161, 184, 202));
}
.body{
  width: 895px;
  margin: 0 auto;
  min-height: 680px;
  /* background: linear-gradient(rgb(72, 86, 99), rgb(161, 184, 202)); */

}
</style>