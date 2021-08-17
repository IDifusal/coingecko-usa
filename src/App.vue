<template>
  <div class="container">
    <div class="row">
      <h1 class="text-center py-3">Coin price 🤠</h1>
      <input
        type="text"
        class="form-contorl my-3 bg-dark text-light"
        placeholder="Search Coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      />
      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">
              {{ title }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-muted">
              {{ index + 1 }}
            </td>
            <td>
              {{ coin.name }}
              <img
                :src="coin.image"
                style="width: 4rem"
                :alt="coin.symbol"
                class="me-2"
              />
              <span class="ms-2 uppercase text-muted">
                {{ coin.symbol }}
              </span>
            </td>
            <td>${{ coin.current_price }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              %{{ coin.price_change_percentage_24h }}
            </td>
            <td>${{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24h var"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    console.log(data);
    this.coins = data;
    this.filteredCoins = data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style>
h1 {
  color: white;
}
</style>
