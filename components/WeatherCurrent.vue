<script setup>
const props = defineProps({
    data: Object
})


</script>

<template>
    <section class="weather-current">
        <h2 class="weather-current-title">
            <span>Погода в городе {{ data.location.name }} сейчас</span>
            <img :src="data.current.condition.icon" :alt="data.current.condition.text">
        </h2>
        <div class="weather-current-icon">
        </div>
        <div class="weather-current-body">
            <div class="weather-current-item">
                <span class="weather-current-key">Температура: </span>
                <span class="weather-current-value">{{ data.current.temp_c }}°C</span>
            </div>
            <div class="weather-current-item">
                <span class="weather-current-key">Влажность: </span>
                <span class="weather-current-value">{{ data.current.humidity }}%</span>
            </div>
            <div class="weather-current-item">
                <span class="weather-current-key">Скорость ветра: </span>
                <span class="weather-current-value">{{ (data.current.wind_kph * 0.28).toFixed(2) }} м/с</span>
            </div>
            <div class="weather-current-item">
                <span class="weather-current-key">Обновлено: </span>
                <span class="weather-current-value">{{ data.current.last_updated }}</span>
            </div>
        </div>
    </section>
</template>

<style lang="scss" scoped>
.weather-current {
    width: 100%;
    background: #f4f5f1;
    border-radius: 10px;
    padding: 20px;
    &-title {
        text-transform: uppercase;
        font-weight: 700;
        font-size: 24px;
        display: flex;
        align-items: center;
        gap: 15px;
        @media(max-width: 767px) {
            font-size: 20px;;
        }
    }
    &-body {
        display: grid;
        grid-template-columns: 1fr 1fr;
        column-gap: 10px;
        font-size: 20px;
        @media(max-width: 767px) {
            grid-template-columns: 1fr;
            font-size: 16px;
        }
    }
    &-item {
        padding: 10px 0;
        display: flex;
        align-items: center;
        column-gap: 15px;
        @media(max-width: 767px) {
            justify-content: space-between;
            &:nth-child(even) {
                background: #ddd;
            }
        }
    }
    &-value {
        font-weight: 700;
        white-space: nowrap;
    }
}

</style>