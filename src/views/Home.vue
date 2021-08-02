<template>
  <main class="mb-4">
    
    <DataTitle :dataDate="dataDate" :text="title" />
    
    <DataBoxes :stats="stats" />
    
    <CountrySelect @get-country="getCountryData" :countries="countries" />

 <button
      v-if="stats.Country"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600"
      @click="clearCountryData"
    >
      Clear Country
    </button>
  </main>
</template>

<script>
  import CountrySelect from '@/components/CountrySelect';
  import DataTitle from '@/components/DataTitle';
  import DataBoxes from '@/components/DataBoxes';
  export default {
    name: 'Home',
    components: {
      DataTitle,
      DataBoxes,
      CountrySelect
    },
    data() {
      return {
        loading: true,
        title : 'Global',
        dataDate: '',
        stats : {},
        countries: [],

      }
    },
    methods: {
      async fetchCovidData() {
        const res = await fetch('https://api.covid19api.com/summary');
        const data = await res.json();
        return data;
      },
      getCountryData(country) {
        this.stats = country
        this.title = country.Country
      },
      async clearCountryData()  {
        const data = await this.fetchCovidData();
        this.title = 'Global';
        this.stats = data.Global;
      }
    },
    async created() {
      const data = await this.fetchCovidData()

      this.dataDate = data.Date
      this.stats = data.Global
      this.countries = data.Countries
      this.loading = false
    },
  }
</script>