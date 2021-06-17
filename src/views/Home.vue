<template>
  <main v-if="!loading">
    <DataTitle :text="title" :dataDate="dataDate" />
    <DataBoxes :stats="stats" />
    <CountrySelect @get-country="getCountryData" :countries="countries" />
    <button
      @click="clearCountryData"
      v-if="stats.Country"
      class="
        bg=green-700
        text-white
        rounded
        p-3
        mt-10
        focus:outline-none
        hover:bg-green-600
      "
    >
      Clear country
    </button>
  </main>

  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">Fetching data</div>
    <img :src="loadingImage" class="w-24 m-auto" alt="Fetching data" />
  </main>
</template>

<script>
import DataTitle from "@/components/DataTitle";
import DataBoxes from "@/components/DataBoxes";
import CountrySelect from "@/components/CountrySelect";

export default {
  name: "Home",
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  data() {
    return {
      loading: true,
      title: "Global",
      dataDate: "",
      stats: {},
      countries: [],
      loadingImage: require("../assets/hourglass.gif"),
    };
  },
  methods: {
    async fetchCovidData() {
      //just call API
      const res = await fetch("https://api.covid19api.com/summary");
      const data = res.json();
      return data;
    },
    getCountryData(country) {
      this.stats = country;
      this.title = country.Country;
    },
    async clearCountryData() {
      loading.value = true;
      const data = await fetchCovidData();
      title.value = "Global";
      status.value = data.Global;
      loading.value = false;
    },
  },
  async created() {
    const data = await this.fetchCovidData();

    //populate data structure from API call
    this.dataDate = data.Date;
    this.stats = data.Global;
    this.countries = data.Countries;
    this.loading = false;
  },
};
</script>
