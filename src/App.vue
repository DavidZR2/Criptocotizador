<template>
  <div class="container">
    <h1>Cotizador de Criptomonedas</h1>

    <Grid>
      <formulario @info-moneda="obtener" />
      <Data :cripto="info.cripto" :moneda="info.moneda" :img="info.img" :precio="info.precio" :symbol="info.symbol" />
    </Grid>

  </div>
  <reload-prompt />
</template>

<script>
import Formulario from './components/Formulario.vue';
import Data from './components/Data.vue';
import Grid from './components/Grid.vue';
import ReloadPrompt from './components/ReloadPrompt.vue';
export default {
  components: {
    Formulario,
    Data,
    Grid,
    ReloadPrompt,
},
  data: () => ({
    info: {
      cripto: "*",
      moneda: "*",
      img: "/media/37746238/eth.png",
      precio: 0,
      symbol: "$",
    },
  }),
  methods: {
    async obtener(cripto, moneda) {
      const res = await fetch([`https://min-api.cryptocompare.com/data/pricemultifull?fsyms=${encodeURI(cripto)}&tsyms=${encodeURI(moneda)}`]);
      const {RAW, DISPLAY} = await res.json();

      const info = RAW[cripto];
      const sym = DISPLAY[cripto];
      const smb = sym[moneda];
      const data = info[moneda];
      
      this.info.cripto = cripto;
      this.info.moneda = moneda;
      this.info.img = data.IMAGEURL;
      this.info.precio = data.PRICE;
      this.info.symbol = smb.TOSYMBOL;
    }
  },
};

</script>