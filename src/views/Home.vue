<template>
 <div>
  <v-layout :wrap="true"> <!--row-->
    <v-flex xs12><!-- xs -> small 6 columns-->
      <v-card >
        <v-date-picker v-model="fecha"
        full-width
        locale = "es"
        :min="minimo"
        :max="maximo"
        @change="getDolar(fecha)"
        >
        </v-date-picker>
      </v-card>

      <v-card color="error" dark> 
        <v-card-text class="display-1 text-xs-center">
          {{valor}}
        </v-card-text>
      </v-card>
    </v-flex>

    
  </v-layout>

  
 </div>
</template>

<script>
import axios from "axios"; 
// @ is an alias to /src

export default {
  data(){
    return {
      fecha: '',
      minimo: '1984',
      maximo: new Date().toISOString().substr(0,10),
      valor: null
    }
  },
  methods:{
    async getDolar(dia){ //asyn pa poder trabajar con await
      let arrayFecha = dia.split(['-'])
      let ddmmyy = arrayFecha[2]+'-'+arrayFecha[1]+'-'+arrayFecha[0]
      try {
        let datos = await axios.get(`https://mindicador.cl/api/dolar/${ddmmyy}`)
        if(datos.data.serie.length > 0) {
          //HAY VALOR DEL DOLAR
          console.log(datos.data.serie[0].valor);
          this.valor = await datos.data.serie[0].valor*0.0050
        }else{
          this.valor = 'Sin resultados'
        }
        
      }catch(error){
        console.log(error);
      }
      
     
    }
  },
  created(){
    this.getDolar(this.fecha)
  }
  //name: 'Home',
  //fecha:'', 
  //components: {
 
  //}
}
</script>
