<template>
  <div id="app">
    <div class="search">
      <form @submit.prevent="callApi">
        <input type="text" v-model="query" />
        <button>Go</button>
      </form>
    </div>

    <div class="grid-container">
      <div v-for="card in cards" :key="card.id" class="grid-item">
        <img :src="card.card_images[0].image_url" width="150" height="265" />
        <h3>{{card.name}}</h3>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data: () => {
    return {
      api_endPoint: "https://db.ygoprodeck.com/api/v5/cardinfo.php?fname=",
      query: "",
      cards: []
    };
  },
  methods: {
    callApi() {
      this.cards = [];
      if (this.query != "") {
        let request = new Request(this.api_endPoint + this.query);
        fetch(request)
          .then(response => {
            if (response.status === 200) return response.json();
          })
          .then(data => {
            data.forEach(card => {
              this.cards.push(card);
            });
            console.log(data);
            this.query = "";
          });
      }
    }
  }
};
</script>

<style>
body {
  background: linear-gradient(
    54deg,
    rgba(2, 0, 36, 1) 0%,
    #c31432 33%,
    #240b36 100%
  );
}
.grid-container {
  grid-template-columns: repeat(auto-fit, minmax(414px, 1fr));
  display: grid;
  background-color: transparent;
  padding: 5px;
  grid-gap: 5px;
  width: 100%;
}
.grid-item {
  background-color: transparent;
  border: 1px solid rgba(0, 0, 0, 0.8);
  padding: 20px;
  font-size: 15px;
  text-align: center;
  border-radius: 5px;
  cursor: pointer;
  color: aliceblue;
  box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2), 0 10px 10px rgba(0, 0, 0, 0.2);
}
.search {
  margin-left: auto;
  margin-right: auto;
}
</style>
