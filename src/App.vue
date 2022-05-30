<template>
  <div class="wrapper">
      <div id="weather" class="weather">
        
      </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            weather: []
        }
    },
    methods: {
        async loadWeather() {
            // const server = axios.get('https://api.openweathermap.org/data/2.5/weather?units=metric&q=Lutsk&appid=c154ff1f8b740d23f2ed0b5cd246b68e')
            // .then(res => {
            //     this.weather = res.data
            // })
            // console.log('https://api.openweathermap.org/data/2.5/weather?units=metric&q=Lutsk&appid=c154ff1f8b740d23f2ed0b5cd246b68e')
            const server = 'https://api.openweathermap.org/data/2.5/weather?units=metric&q=Lutsk&appid=c154ff1f8b740d23f2ed0b5cd246b68e'
            const response = await fetch(server, {
                method: 'GET',
            })
            const responseResult = await response.json();
            if (response.ok) {
                this.getWeather(responseResult)
            } else {
                alert('Фу лох')
            }
        },
        getWeather(data) {
            const weatherBlock = document.querySelector('#weather')
            const location = data.name;
            const temp = Math.round(data.main.temp);
            const feelslike = Math.round(data.main.feels_like);
            const weatherStatus = data.weather[0].main;
            const weatherIcon = data.weather[0].icon;

            const template = 
            `<div class="weather__header">
              <div class="weather__main">
                  <div class="weather__city">${location}</div>
                  <div class="weather__status">${weatherStatus}</div>
              </div>
              <div class="weather__icon">
                <img src='https://openweathermap.org/img/w/${weatherIcon}.png' alt=${weatherStatus}
              </div>
          </div>
          <div class="weather__temp">Tемпература: ${temp}</div>
          <div class="weather__feel">Відчувається: ${feelslike}</div>`

            weatherBlock.innerHTML = template

        }
    },
    mounted() {
        this.loadWeather()
    }
}
</script>

<style lang="scss">
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
#app{

}
.wrapper{
    display: flex;
    justify-content: center;
    padding: 200px 10px 0;
    height: 100vh;
    background-color: rgb(9, 9, 24);
}
.weather{
    max-width: 800px;
    padding: 30px;
    width: 100%;
    height: fit-content;
    background-color: #ffffff59;
    border-radius: 20px;
    margin-bottom: 15px;
    .weather__main{
        display: flex;
        align-items: center;
        font-size: 30px;
        justify-content: space-between;
    }
    .weather__icon{
        margin-left: auto;
        width: 70px;
        height: 70px;
        img{
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
    }
    .weather__temp{
        font-size: 30px;
        display: inline-block;
        margin-right: 30px;
    }
    .weather__feel{
        font-size: 30px;
        display: inline-block;
    }
}
</style>