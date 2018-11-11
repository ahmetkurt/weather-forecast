<template>
  <article class="weather">
    <header class="weather__header">
      <h3 class="weather__heading">{{ weather.name }}</h3>
      <time class="weather__date" :datetime="weather.dt | dateISOString">
        {{ weather.dt | dateLocaleString }}
      </time>
    </header>
    <div class="weather__temperature">
      <span class="weather__value">{{ Math.round(weather.main.temp) }}</span>&nbsp;
      <span class="weather__unit">&deg;C</span>
    </div>
    <span class="weather__description">
      {{ weather.weather.map(x => x.description).join(', ') }}
    </span>
    <span class="weather__wind-speed">Wind Speed: {{ weather.wind.speed }} meter/sec</span>
  </article>
</template>

<script>
export default {
  name: 'Weather',
  props: {
    weather: Object,
  },
  filters: {
    dateISOString(timestamp) {
      return new Date(timestamp * 1000 - new Date()
        .getTimezoneOffset() * 60000)
        .toISOString()
        .slice(0, -1);
    },
    dateLocaleString(timestamp) {
      return new Date(timestamp * 1000).toLocaleString(
        document.documentElement.lang, {
          year: 'numeric',
          month: 'long',
          day: '2-digit',
          hour: '2-digit',
          minute: '2-digit',
        },
      );
    },
  },
};
</script>

<style lang="scss">
.weather {
  display: flex;
  flex-basis: 320px;
  flex-direction: column;
  flex-grow: 1;
  justify-content: space-between;
  padding: 0 0 .5rem .5rem;
  margin: .25rem;
  background-color: #0033ff;
  color: #fff;

  &__header {
    display: flex;
    flex-direction: row-reverse;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 2rem;
  }

  &__heading {
    flex-basis: 55%;
    overflow: hidden;
    margin: 0;
    font-size: 1.5rem;
    font-weight: 600;
    line-height: .833333;
    text-align: right;
    text-overflow: ellipsis;
    text-transform: uppercase;
  }

  &__date {
    flex-basis: 45%;
    font-size: .75rem;
  }

  &__temperature {
    display: flex;
    justify-content: center;
    margin-bottom: .5rem;
  }

  &__value {
    overflow: hidden;
    font-size: 4rem;
    font-weight: 600;
    line-height: .8125;
  }

  &__unit {
    overflow: hidden;
    line-height: .875;
  }

  &__description {
    font-size: .875rem;
    text-align: center;
    text-transform: capitalize;
  }

  &__wind-speed {
    font-size: .75rem;
    text-align: center;
  }
}
</style>
