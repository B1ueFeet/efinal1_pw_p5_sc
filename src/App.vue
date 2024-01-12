<template>
  <div v-if="perdio">
    <h1>Has utilizado tus 5 intentos
El juego ha termindo, intentalo nuevamente</h1>
  </div>
  <div class="tablero">
    <h1>Score {{ score }}</h1>
    <h1>Intento {{ intento }}</h1>
  </div>
  <div class="container">
    <Game ref="g1" />
    <Game ref="g2" />
    <Game ref="g3" />
  </div>
  <button v-on:click="jugar()">{{ txt_jugar }}</button>
</template>

<script>
import Game from './components/Game.vue'
export default {
  name: 'App',
  components: {
    Game
  },
  data() {
    return {
      score: 0,
      intento: 0,
      txt_jugar: "Jugar",
      calcular: false,
      mensaje: null,
      gano: false,
      perdio: false,
    };
  },
  methods: {
    jugar() {
      if (this.intento < 6 && this.score < 11) {
        this.intento++
        this.$refs.g1.consumirApi();
        this.$refs.g2.consumirApi();
        this.$refs.g3.consumirApi();
      }else if (this.intento > 6){
        this.perdio = true;
      }
    },
    calcularScore() {

      const s1 = this.$refs.g1.respuesta === "yes"? 1:0;
      const s2 = this.$refs.g2.respuesta === "yes"? 1:0;
      const s3 = this.$refs.g3.respuesta=== "yes"? 1:0;

      var score_aux = 0;
      if ((s1+s2+s3) === 3) {
        score_aux += 5;
      } else if ((s1+s2+s3) === 2){
        score_aux += 3;
      }else if ((s1+s2+s3) === 2){
        score_aux += 1;
      }
      this.score += score_aux;
    },
  },
  watch: {
    intento(value, oldValue) {
      const s1 = this.$refs.g1.jackpot;
      const s2 = this.$refs.g2.jackpot;
      const s3 = this.$refs.g3.jackpot;

      console.log("Calcular")
      console.log(s1)
      console.log(s2)
      console.log(s3)

      if (s1 && s2 && s3) {
        this.calcularScore()
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  /* Espacio entre las columnas */
}

.tablero {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  /* Espacio entre las columnas */
}
</style>
