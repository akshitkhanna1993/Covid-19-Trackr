<template>
  <div class="home">
    <main>
      <DataTitle  :text="title"  :dataDate="date" />
      <DataBoxes  :stats="stats" />
      <CountrySelect @get-country="getCountryData"  :countries="countries" />
      <button  @click="clearCountryData"  v-if="stats.Country"  class="bg-green-600 text-center    text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-500">
       Clear countries
      </button>

    </main>
    
  </div>
</template>

<script>
import DataTitle from '../components/DataTitle'
import DataBoxes from '../components/DataBoxes'
import CountrySelect from '../components/CountrySelect'
export default {
  name: 'Home',
  components:{
   DataTitle,
   DataBoxes,
   CountrySelect
  },
  data() {
    return {
      loading:true,
      title:'Global',
      date:'',
      stats:{},
      countries:[],

    }
  },
  methods:{
     async fetchCovidData() {
      //let data
     let res =  await fetch('https://api.covid19api.com/summary')
       
        let data = await res.json()

       this.date = data.Date
       this.stats = data.Global
       this.countries = data.Countries

    },
    getCountryData(country){
      console.log(country)
      this.stats = country
      this.title = country.Country
    },
    async clearCountryData() {
      this.title='Global'
     const data = await this.fetchCovidData()
     this.stats = data.Global
    }
  },
  mounted() {
      this.fetchCovidData()
  } ,
  
}


</script>
