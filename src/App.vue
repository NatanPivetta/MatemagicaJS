<script>
import { ref } from "vue"
import axios from "axios"
export default {
  name: "App",
  setup() {
    let gameOver = ref(true),
      cardOne = ref({}),
      cardTwo = ref({}),
      deckID = ref(null);
    var deck = []

    async function getDeck() {
      gameOver.value = false
      if (deckID.value == null) {
        // criar novo deck
        const { data } = await axios.get("https://deckofcardsapi.com/api/deck/new/shuffle/?count=1")
        deckID.value = data.deck_id
        console.log(data)

      }

    } //getDeck()
    async function getCards() {
      if (deck.length == 0) {
        const { data } = await axios.get("https://deckofcardsapi.com/api/deck/" + deckID.value + "/draw/?count=21")

        const remaining = data.cards.remaining
        const { cards } = data

        let i = 0;
        cards.forEach(element => {
          deck[i] = cards[i]
          i++;
        });


        cardOne.value = cards[0]
        cardTwo.value = cards[1]
        console.log(deck)

      }

    }

    function um() {
      // coluna 1(0) escolhida
      // coluna 1(0) ocupara posicao do meio (1)
      let aux = []
      let i = 1
      for (let j = 0; j < 7; j++) {
        
        aux[j] = deck[i];
        deck[j] = aux[j];
        i+=3;
        
      }
      console.table(aux);
      console.table(deck)


    }

    function dois() {

    }

    function tres() {

    }





    return { getDeck, getCards, cardOne, cardTwo, deck, um }
  },

} //export default






</script>

<template>
  <h1>Matemagica</h1>
  <div id="gameBoard">
    <div class="card" v-for="card in deck">
      <img :src="card.image" :alt="card.code" />
    </div>

  </div>
  <div class="navi3">
    <button v-on:click="um()">1</button>
    <button>2</button>
    <button>3</button>
  </div>

  <div class="navi2">
    <button v-on:click="getDeck()">Novo Baralho</button>
    <button v-on:click="getCards()">Iniciar Jogo</button>
  </div>
</template>

<style scoped>
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0px;
  font-weight: normal;
  font-family: "BigSpace";

}

@font-face {
  font-family: "BigSpace";
  src: local("BigSpace"), url("./assets/BigSpace.ttf") format("truetype");
}

h1 {
  text-align: center;
}

.navi3 {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 90vw;
  height: auto;
  display: grid;
  align-items: center;
  justify-content: space-around;
  grid-template-columns: 90px 90px 90px;

}

.navi2 {
  display: flex;
  justify-content: space-between;
  width: 90vw;
  height: auto;
  display: grid;
  justify-content: space-around;
  grid-template-columns: 90px 90px;
  margin-top: 10px;
}



#gameBoard {
  width: 90vw;
  height: auto;
  display: grid;
  align-items: center;
  justify-content: space-around;
  grid-template-columns: 90px 90px 90px;
  grid-template-rows: repeat(7, 80px);
  gap: 0px 0px;
  grid-column-start: 1;
  grid-column-end: 3;



}

.card {
  display: flex;
  justify-content: center;
  align-items: center;
  scale: .75;
  width: 100px;
  height: 100px;
}

.card img {
  max-width: 100%;
  max-height: 100%;
}
</style>
