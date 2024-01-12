<template>
    <div>
        <img v-if="img" v-bind:src="img" alt="No se puede visualizar la imagen" />
        <h1>{{ respuesta }}</h1>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            respuesta: null,
            image: null,
            jackpot: false,
        };
    },
    methods: {
        async consumirApi() {
            this.think()
            const { answer, image } = await fetch("https://yesno.wtf/api").then(
                (respuesta) => respuesta.json()
            );
            console.log(answer);
            console.log(image);
            this.respuesta = answer;
            this.img = image;
        },
        async think() {
            this.respuesta = "pensando."
            this.respuesta = "pensando.."
            this.respuesta = "pensando..."
            this.respuesta = "pensando...."
        }
    },
    watch: {
        respuesta(value, oldValue) {
            console.log(value);
            console.log(oldValue);
            if(this.respuesta === "yes" || this.respuesta ==="no"){
                this.jackpot = true;
            }else{
                this.jackpot = false;
            }
            console.log(this.jackpot)
        }
    }

};
</script>
  
<style>
img {
    height: 300px;
    width: 300px;
}
</style>