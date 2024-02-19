<script setup>
useHead({
  title: 'weather-app'
})

const city = ref('');
let loading = ref(false);
let error = ref('');
let data = ref(null)

const inputRef = ref(null)

const showCityWeather = async () => {
  try {
      loading.value = true;
      if(!city.value) {
        throw new Error('Не указан город')
      }
      if(city.value === data.value?.location.name) {
        return;
      } else {
          data.value = await $fetch('https://weatherapi-com.p.rapidapi.com/forecast.json', {
              query: {
                  q: city.value,
                  days: 3
              },
              headers: {
                  'X-RapidAPI-Key': '5eb5292671msh290f6e09a88323ep1a4efbjsn7a214d9112ef',
                  'X-RapidAPI-Host': 'weatherapi-com.p.rapidapi.com'
              }
          })
      }
  } catch(e) {
      if (e.name === 'FetchError') {
        if(data.value) {
          data.value = null;
        }
        error.value = 'Запрошенный город не найден...'
      } else {
        error.value = e.message;
      }
  } finally {
    loading.value = false;
  }
}

const handleKeyPress = (e) => {
  if(e.code === 'Enter' && inputRef.value === document.activeElement) {
    showCityWeather()
  }
}

onMounted(() => {
  window.addEventListener('keypress', handleKeyPress)
})

onBeforeUnmount(() => {
  window.removeEventListener('keypress', handleKeyPress)
  data.value = null;
})

</script>

<template>
  <div class="weather">
    <div class="weather-container">
      <h1 class="weather-title">Узнай погоду в любом городе планеты</h1>
      <div class="weather-input">
        <input ref="inputRef" type="text" v-model="city" placeholder="Город (латиницей/кириллицей)">
      </div>
      <button class="weather-btn" @click="showCityWeather">Показать</button>
      <div class="weather-views">
        <div v-if="loading" class="weather-loading">
          <img src="~/public/icons/loading.svg" alt="loading">
        </div>
        <div v-else-if="data" class="weather-views">
          <weather-current class="weather-views-current" :data="data" />
          <weather-forecast :data="data" />
        </div>
        <div v-else-if="error" class="weather-error">
            {{ error }}
        </div> 
      </div>
    </div>
    
  </div>
</template>

<style lang="scss" scoped>
@import '~/public/scss/style';
.weather {
  min-width: 100%;
  min-height: 100%;
  height: fit-content;

  &-container {
    margin: 0 auto;
    max-width: 1200px;
    padding: 50px 15px;
    display: flex;
    flex-direction: column;
  }

  &-title {
    text-align: center;
    margin: 0 auto;
    text-transform: uppercase;
    font-weight: 700;
    font-size: 54px;
    margin-bottom: 30px;
    max-width: 500px;
    @media(max-width: 991px) {
      font-size: 36px;
    }@media(max-width: 767px) {
      font-size: 28px;
    }
  }

  &-input {
    width: 100%;
    margin-bottom: 30px;
    input {
      display: flex;
      align-items: center;
      margin: 0 auto;
      padding-inline: 20px;
      width: 100%;
      max-width: 500px;
      border-radius: 10px;
      min-height: 50px;
      background: #f4f5f1;
      outline: none;
      transition: all 0.3s ease 0s;
      &:focus {
        box-shadow: 0 2px 5px $mainColor;
        &::placeholder {
          opacity: 0;
        }
      }
    }
  }

  &-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    width: fit-content;
    min-height: 50px;
    margin: 0 auto 50px;
    padding-inline: 35px;
    color: #f4f5f1;
    border-radius: 10px;
    background: $mainColor;
    font-weight: 700;
    transition: all 0.3s ease 0s;
    @media(any-hover) {
      &:hover {
        color: $mainColor;
        background: #f4f5f1;
      }
    }
  }

  &-views {
    flex: auto;
    &-current {
      margin-bottom: 30px;
    }
  }

  &-loading {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 200px;
  }

  &-error {
    color: red;
  }
}

</style>
