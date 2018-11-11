<template>
  <main class="main">
    <section class="section section--weather">
      <Weather v-for="item in list" :key="item.id" :weather="item" />
    </section>
  </main>
</template>

<script>
import axios from 'axios';
import Weather from './components/Weather.vue';

const apiUrl = 'http://api.openweathermap.org/data/2.5/';
const apiAppId = '2d753d7a2080ca15564eafd2e2323c29';

const citiesEnum = Object.freeze({
  istanbul: 745044,
  zurich: 2657896,
  vaduz: 3042030,
  copenhagen: 2618425,
  london: 2643743,
  edinburgh: 2650225,
  cardiff: 2653822,
});

export default {
  name: 'app',
  components: {
    Weather,
  },
  data() {
    return {
      cityIds: [
        citiesEnum.istanbul,
        citiesEnum.zurich,
        citiesEnum.vaduz,
        citiesEnum.copenhagen,
        citiesEnum.london,
        citiesEnum.edinburgh,
        citiesEnum.cardiff,
      ],
      units: 'metric',
      list: {},
    };
  },
  mounted() {
    this.get();
  },
  methods: {
    get() {
      const vm = this;

      axios
        .get(`${apiUrl}group?id=${vm.cityIds.join(',')}&units=${vm.units}&appid=${apiAppId}`)
        .then((response) => {
          const responseData = response.data;

          if (responseData) vm.list = responseData.list;
        });
    },
  },
};
</script>

<style lang="scss">
*,
*::before,
*::after {
  box-sizing: border-box;
}

.body {
  margin: .25rem;
  font-family: 'Poppins', sans-serif;
}

.section {
  &--weather {
    display: flex;
    flex-wrap: wrap;
  }
}
</style>
