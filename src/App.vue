<template>
  <div class="container" v-if="showGame">
    <div class="head">
      <h1>Casino Pokemon</h1>
      <h2>Puntaje: {{ puntaje }}</h2>
      <h2>Intento: {{ intento }}</h2>
    </div>
    <Game :texto="text1" :urlImg="url1" />
    <Game :texto="text2" :urlImg="url2" />
    <Game :texto="text3" :urlImg="url3" />
    <button class="jugar" @click="jugar">Jugar</button>
  </div>
  <div class="win" v-if="showWin">
    <h1>Puntaje: {{ puntaje }}</h1>
    <h1>Felicitaciones has ganado un premio de $10.000,00</h1>
    <button class="new_game" @click="reiniciar">Nuevo Juego</button>
  </div>
  <div class="loose" v-if="showLooser">
    <h1>Hasutilizado tus 5 intentos</h1>
    <h1>El juego ha termindo, intentalo nuevamente</h1>
    <button class="new_game" @click="reiniciar">Nuevo Juego</button>
  </div>
</template>

<script>
import Game from "./components/Game.vue";
export default {
  name: "App",
  components: {
    Game,
  },
  data() {
    return {
      puntaje: 0,
      intento: 0,
      url1: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
      url2: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
      url3: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
      text1: "xxxxxxxxxx",
      text2: "xxxxxxxxxx",
      text3: "xxxxxxxxxx",
      showWin: false,
      showLooser: false,
      showGame: true,
    };
  },
  methods: {
    async jugar() {
      var data = [
        await this.consumirAPI(),
        await this.consumirAPI(),
        await this.consumirAPI(),
      ];
      this.text1 = data[0].answer;
      this.text2 = data[1].answer;
      this.text3 = data[2].answer;
      this.url1 = data[0].image;
      this.url2 = data[1].image;
      this.url3 = data[2].image;

      this.evaluarResultado();
    },
    async consumirAPI() {
      return await fetch("https://yesno.wtf/api").then((r) => r.json());
    },
    evaluarResultado() {
      this.intento++;
      var aux = 0;

      if (this.text1 === "yes") aux++;
      if (this.text2 === "yes") aux++;
      if (this.text3 === "yes") aux++;

      if (aux === 3) this.puntaje += 5;
      if (aux === 2) this.puntaje += 2;
      if (aux === 1) this.puntaje += 1;

      if (this.puntaje >= 10) {
        this.showWin = true;
        this.showLooser = false;
        this.showGame = false;
      }
      if (this.intento >= 5) {
        this.showLooser = true;
        this.showWin = false;
        this.showGame = false;
      }
    },
    reiniciar() {
      this.puntaje = 0;
      this.intento = 0;
      this.url1 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg";
      this.url2 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg";
      this.url3 =
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg";
      this.text1 = "xxxxxxxxxx";
      this.text2 = "xxxxxxxxxx";
      this.text3 = "xxxxxxxxxx";
      this.showWin = false;
      this.showLooser = false;
      this.showGame = true;
    },
  },
};
</script>

<style>
img {
  height: 200px;
  width: 200px;
}

body{
  font-size: 20px;
}

.container {
  display: grid;
  grid-template-columns: repeat(3, 300);
  justify-content: center;
  background: whitesmoke;
  width: auto;
  justify-content: center;
  text-align: center;
  margin: 50px;
  border-radius: 15px;
  padding: 30px;
}
.head {
  grid-column: span 3;
  display: grid;
  grid-template-columns: repeat(2, 450px);
}
h1,
.jugar {
  grid-column: span 3;
}

.win,
.loose {
  background: whitesmoke;
  width: auto;
  justify-content: center;
  text-align: center;
  margin: 50px;
  border-radius: 15px;
  padding: 30px;
}

.win {
  color: blue;
}

.new_game {
  width: 300px;
}

button{
    margin: 30px 10px;
    padding: 20px;
    font-size: 30px;
    font-weight: bold;
}

.loose {
  color: red;
}
</style>
