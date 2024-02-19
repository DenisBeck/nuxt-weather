<script setup>
defineProps({
    data: Object
})
</script>

<template>
    <section class="weather-forecast">
        <h2 class="weather-forecast-title">
            Погода в городе {{ data.location.name }} в ближайшие три дня
        </h2>
        <ul class="weather-forecast-list">
            <li class="weather-forecast-listitem" v-for="day in data.forecast.forecastday" :key="day.date">
                <div class="weather-forecast-date">
                    {{ day.date }}
                </div>
                <div class="weather-forecast-icon">
                    <img :src="day.day.condition.icon" :alt="day.day.condition.text">
                </div>
                <div class="weather-forecast-item">
                    <span class="weather-forecast-key">Температура: </span>
                    <span class="weather-forecast-value">{{ day.day.avgtemp_c }}°C</span>
                </div>
                <div class="weather-forecast-item">
                    <span class="weather-forecast-key">Влажность: </span>
                    <span class="weather-forecast-value">{{ day.day.avghumidity }}%</span>
                </div>
                <div class="weather-forecast-item">
                    <span class="weather-forecast-key">Скорость ветра: </span>
                    <span class="weather-forecast-value">{{ (day.day.maxwind_kph * 0.28).toFixed(2) }} м/с</span>
                </div>
            </li>
        </ul>
    </section>
</template>

<style lang="scss" scoped>
.weather-forecast {
    width: 100%;
    background: #f4f5f1;
    border-radius: 10px;
    padding: 20px;
    &-title {
        text-transform: uppercase;
        font-weight: 700;
        font-size: 24px;
        padding-top: 20px;
        margin-bottom: 30px;
        @media(max-width: 767px) {
            font-size: 20px;;
        }
    }
    &-list {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        gap: 20px;
        @media(max-width: 767px) {
            flex-direction: column;
            align-items: stretch;
            gap: 60px;
        }
    }
    &-listitem {
        flex: auto;
        max-width: 260px;
        @media(max-width: 767px) {
            max-width: 100%;
        }
    }
    &-item {
        padding: 10px 0;
        display: flex;
        align-items: center;
        justify-content: space-between;
        @media(max-width: 767px) {
            &:nth-child(odd) {
                background: #ddd;
            }
        }
    }
    &-date {
        font-weight: 700;
        text-align: center;
        margin-bottom: 10px;
    }
    &-icon {
        text-align: center;
    }
    
    &-value {
        font-weight: 700;
        white-space: nowrap;
    }
}


</style>