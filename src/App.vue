<template>
  <div id="app">
    <img src="./assets/cronometro.png" class="img">
    <p class="timer">{{numero}}</p>
    <div class="btns">
      <button @click="iniciar">{{btnIniciar}}</button>
      <button @click="reiniciar">Reiniciar</button>
    </div>

    <div class="list" v-show="historico.length > 0">
      <ul>
        <li v-for="item in historico" :key="item">Você fez uma pausa em {{item}}</li>
      </ul>
      <button @click="historico = []">Limpar histórico</button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data(){
      return {
        numero: '00:00:00',
        btnIniciar: 'Iniciar',
        timer: null,
        horas: 0,
        minutos: 0,
        segundos: 0,
        historico: []
      }
    },
    methods: {
      iniciar(){
        if(this.timer !== null){
          clearInterval(this.timer)
          this.timer = null
          this.btnIniciar = 'Iniciar'
          this.historico.push(this.numero)
        }else{
          this.timer = setInterval(() => {
            this.rodarTimer()
          }, 1000)
          this.btnIniciar = 'Parar'
        }
      },
      
      reiniciar(){
        if(this.timer !== null){
          clearInterval(this.timer)
          this.timer = null
          this.horas = 0
          this.minutos = 0
          this.segundos = 0
          this.numero = '00:00:00'
          this.btnIniciar = 'Iniciar'
          this.historico = []
        }
      }, 

      rodarTimer(){
        let format

        this.segundos++

        if(this.segundos == 59){
          this.minutos++
          this.segundos = 0
        }

        if(this.minutos == 59){
          this.horas++
          this.minutos = 0
        }

        format  = (this.horas < 10 ? `0${this.horas}` : this.horas) + ':'
        format += (this.minutos < 10 ? `0${this.minutos}` : this.minutos) + ':'
        format += (this.segundos < 10 ? `0${this.segundos}` : this.segundos)

        this.numero = format
      }
    }
  }
</script>

<style>
  #app {
    display: flex;
    width: 100%;
    align-items: center;
    justify-content: center;
    flex-direction: column;
  }

  .img {
    width:420px;
    height:420px;
    padding-top: 100px;
  }

  .timer {
    font-size: 60px;
    margin-top:-210px;
  }

  .btns {
    margin-top:155px;
    display: flex;
    gap:10px;
  }

  .btns > button {
    width: 150px;
    padding-top:10px;
    padding-bottom:10px;
    background-color:white;
    border:0; 
    cursor:pointer;
    font-size:16px;
    font-family: 'Courier New', Courier, monospace;
    font-weight: 800;
  }

  .btns > button:hover {
    opacity: 0.8;
    transition:all 0.3s;
  }

  .list > ul{
    text-align:center;
    padding:0px;
    list-style: none;
  }

  .list > ul > li {
    margin-top:4px;
    background-color:rgb(70,70,70);
    padding:15px;
    font-size:16px;
    border-radius:6px;
  }

  .list > button {
    cursor:pointer;
    border:0px;
    background-color: white;
    padding:7px;
    border-radius:6px;
  }

</style>