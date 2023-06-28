<template>
  <div>

    <div v-if="deck">
      <h1>Matemagica</h1>
      <div class="card-grid">
        <div v-for="card in cards" :key="card.code" class="card">
          <img :src="card.image" :alt="card.code" class="card-image" />
        </div>
      </div>
      <div class="buttonDiv">
        <button @click="one">1</button>
        <button @click="two">2</button>
        <button @click="three">3</button>
      </div>

    </div>
    <div v-else>
      <h1>Matemágica</h1>
      <div class="msg">
        <p>Bem vindo</p>
        <p>Ao iniciar o jogo, você receberá 21 cartas</p>
        <p>Escolha uma, e anote em um papel, ou a memorize</p>
        <p>Após escolher a carta, indique em qual coluna ela aparece</p>
        <p>Ao final, o Matemágico adivinhará qual é a sua carta</p>
        <button @click="getDeck">Iniciar Novo Jogo</button>
      </div>
    </div>

    <div v-if="contador == 3">
      <div class="msg">
        <h2>Sua Carta é:</h2>
      </div>
      <div :key="cards[10].code" class="card">
        <img :src="cards[10].image" :alt="cards[10].code">
      </div>
    </div>


  </div>
</template>

<style>
@font-face {
  font-family: "BigSpace";
  src: local("BigSpace"), url("./assets/BigSpace.ttf") format("truetype");
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0px;
  font-weight: normal;
  font-family: "BigSpace";
  color: var(--color-text);

}

.msg p {
  text-shadow: 1px 1px 2px black;
  color: var(--bttn-color-text);
  font-size: 1.3em;
}

div{
  display: inline;
  justify-content: center;
  text-align: center;

}

h1{
  background-color: var(--color-background-mute);
  text-align: center;
  text-shadow: 1px 2px 10px black;
}

.buttonDiv{
  margin: 0px 0px;
  width: 90vw;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: var(--color-text);
}

button{
  background-color: var(--vt-c-indigo);
  height: 60px;
  width: 60px;
  color: var(--bttn-color-text);
}

.card-grid {
  
  width: 90vw;
  height: 90vh;
  display: grid;
  align-items: center;
  justify-content: center;
  grid-template-rows: repeat(7, 1fr);
  grid-template-columns: repeat(3, 1fr);
  gap: 0px 80px;
}

.card {
  scale: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1px;
}

.card-image {
  width: 100%;
  max-width: 150px;
  height: auto;
}
</style>

<script>
export default {
  data() {
    name: "App"
    return {
      deck: null,
      cards: [],
      contador: 0
    };
  },
  methods: {
    async getDeck() {
      if (this.deck == null) {
        try {
          this.contador = 0;
          const response = await fetch('https://deckofcardsapi.com/api/deck/new/draw/?count=21');
          const data = await response.json();
          this.deck = data.deck_id;
          this.cards = data.cards;
          console.table(this.cards)
        } catch (error) {
          console.error(error);
        }
      }


    },
    async one() {

      let column1 = []
      let column2 = []
      let column3 = []
      let i = 0;
      let j = 0;
      for (i = 0; i < 21; i += 3) {
        column1[j] = this.cards[i];
        column2[j] = this.cards[i + 1];
        column3[j] = this.cards[i + 2];
        j++;
      }


      for (i = 0; i < 7; i++) {
        this.cards[i] = column2[i]
        this.cards[i + 7] = column1[i]
        this.cards[i + 14] = column3[i]
      }
      this.iterador()



    },
    async two() {

      let column1 = []
      let column2 = []
      let column3 = []
      let i = 0;
      let j = 0;
      for (i = 1; i < 21; i += 3) {
        column2[j] = this.cards[i]
        column1[j] = this.cards[i - 1]
        column3[j] = this.cards[i + 1]
        j++;
      }

      for (i = 0; i < 7; i++) {
        this.cards[i] = column1[i]
        this.cards[i + 7] = column2[i]
        this.cards[i + 14] = column3[i]
      }
      this.iterador()
    },
    async three() {

      let column1 = []
      let column2 = []
      let column3 = []
      let i = 0;
      let j = 0;
      for (i = 2; i < 21; i += 3) {
        column3[j] = this.cards[i]
        column2[j] = this.cards[i - 1]
        column1[j] = this.cards[i - 2]
        j++;
      }



      for (i = 0; i < 7; i++) {
        this.cards[i] = column1[i]
        this.cards[i + 7] = column3[i]
        this.cards[i + 14] = column2[i]
      }
      this.iterador()

    },

    async iterador() {
      this.contador += 1;
      console.log(this.contador)
      if (this.contador == 3) {
        this.deck = null
        console.log(this.cards[10])
      }


    },




  }
};
</script>
