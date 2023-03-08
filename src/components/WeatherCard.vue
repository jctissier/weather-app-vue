<script setup>
function weather_img(code) {
    // API codes - https://www.weatherapi.com/docs/weather_conditions.json
    if (code == 1000) {
        return 'clear'
    }
    else if (code == 1030) {
        return 'mist'
    }
    else if ([1003, 1006, 1009].includes(code)) {
        return 'cloud'
    }
    else if ([1063, 1180, 1183, 1186, 1189, 1192, 1995, 1198, 1201, 1240, 1243, 1246, 1273, 1276].includes(code)) {
        return 'rain'
    }
    else if ([1210, 1213, 1216, 1219, 1222, 1225, 1255, 1258].includes(code)) {
        return 'snow'
    }
    else {
        return ''
    }
}


function get_weather() {
    const container = document.querySelector('.container');
    const weatherBox = document.querySelector('.weather-box');
    const weatherDetails = document.querySelector('.weather-details');
    const city = document.querySelector('.search-box input').value;

    const APIKey = '124f51569b574f2e939131853230703';

    if (city === '') {
        return;
    }
    
    fetch(`https://api.weatherapi.com/v1/current.json?key=${APIKey}&q=${city}`)
        .then(response => response.json())
        .then(weather => {
            console.log(weather);       // debug API values

            const image = document.querySelector('.weather-box img');
            const temperature = document.querySelector('.weather-box .temperature');
            const description = document.querySelector('.weather-box .description');
            const humidity = document.querySelector('.weather-details .humidity span');
            const wind = document.querySelector('.weather-details .wind span');

            if (weather.hasOwnProperty('error')) {
                temperature.style.display = 'none'
                weatherDetails.style.display = 'none';
                
                image.src = '/404.png';
                description.innerHTML = `${weather.error.message}`;
                container.style.height = '350px';

            } else {
                // weather card API values
                image.src = `/${weather_img(weather.current.condition.code)}.png`;
                
                temperature.innerHTML = `${parseInt(weather.current.temp_c)}<span>°C</span>`;
                temperature.style.display = ''
                description.innerHTML = `${weather.current.condition.text}`;
                
                humidity.innerHTML = `${weather.current.humidity}%`;
                wind.innerHTML = `${parseInt(weather.current.wind_kph)}Km/h`;

                weatherDetails.style.display = '';
                weatherDetails.classList.add('fadeIn');
                
                container.style.height = '600px'; 
            }

            weatherBox.style.display = '';
            weatherBox.classList.add('fadeIn');
        })
}
</script>

<template>
    <div class="container">

        <div class="search-box">
            <i class="fa-solid fa-location-dot"></i>
            <input type="text" placeholder="Enter your location">
            <button @click="get_weather" class="fa-solid fa-magnifying-glass"></button>
        </div>

        <!-- removed 'not-found' section -->

        <div class="weather-box">
            <img src="">
            <p class="temperature"></p>
            <p class="description"></p>
        </div>

        <div class="weather-details">
            <div class="humidity">
                <i class="fa-solid fa-water"></i>
                <div class="text">
                    <span></span>
                    <p>Humidity</p>
                </div>
            </div>
            <div class="wind">
                <i class="fa-solid fa-wind"></i>
                <div class="text">
                    <span></span>
                    <p>Wind Speed</p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .container{
    position: relative;
    width: 400px;
    height: 105px;
    background: #fff;
    padding: 28px 32px;
    overflow: hidden;
    border-radius: 18px;
    font-family: 'Roboto', sans-serif;
    transition: 0.6s ease-out;
    }

    .search-box{
    width: 100%;
    height: min-content;
    display: flex;
    align-items: center;
    justify-content: space-between;
    }

    .search-box input{
    color: #06283D;
    width: 80%;
    font-size: 24px;
    font-weight: 500;
    text-transform: uppercase;
    padding-left: 32px;
    }

    .search-box input::placeholder{
    font-size: 20px;
    font-weight: 500;
    color: #06283D;
    text-transform: capitalize;
    }

    .search-box button{
    cursor: pointer;
    width: 50px;
    height: 50px;
    color: #06283D;
    background: #dff6ff;
    border-radius: 50%;
    font-size: 22px;
    transition: 0.4s ease;
    }

    .search-box button:hover{
    color: #fff;
    background: #06283D;
    }

    .search-box i{
    position: absolute;
    color: #06283D;
    font-size: 28px;
    }

    .weather-box{
    text-align: center;
    }

    .weather-box img{
    width: 60%;
    margin-top: 30px;
    }

    .weather-box .temperature{
    position: relative;
    color: #06283D;
    font-size: 4rem;
    font-weight: 800;
    margin-top: 30px;
    margin-left: -16px;
    }

    .weather-box .temperature span{
    position: absolute;
    margin-left: 4px;
    font-size: 1.5rem;
    }

    .weather-box .description{
    color: #06283D;
    font-size: 22px;
    font-weight: 500;
    text-transform: capitalize;
    }

    .weather-details{
    width: 100%;
    display: flex;
    justify-content: space-between;
    margin-top: 30px;
    }

    .weather-details .humidity, .weather-details .wind{
    display: flex;
    align-items: center;
    width: 50%;
    height: 100px;
    }

    .weather-details .humidity{
    padding-left: 20px;
    justify-content: flex-start;
    }

    .weather-details .wind{
    padding-right: 20px;
    justify-content: flex-end;
    }

    .weather-details i{
    color: #06283D;
    font-size: 26px;
    margin-right: 10px;
    margin-top: 6px;
    }

    .weather-details span{
    color: #06283D;
    font-size: 22px;
    font-weight: 500;
    }

    .weather-details p{
    color: #06283D;
    font-size: 14px;
    font-weight: 500;
    }

    .weather-box, .weathecr-details{
    scale: 0;
    opacity: 0;
    }

    .fadeIn{
    animation: 0.5s fadeIn forwards;
    animation-delay: 0.5s;
    }

    @keyframes fadeIn{
    to {
        scale: 1;
        opacity: 1;
    }
    }
</style>
