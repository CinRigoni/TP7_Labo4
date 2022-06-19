<template>
  <p style="text-align: left; padding-left: 200px;">
      <a class="btn btn-primary" :href="'/formulario/0'" role="button">Nuevo</a>
  </p>  
  <div class="container">
      <div class="row">
        <div class="col">
        <b>ID</b>
        </div>
        <div class="col">
        <b>Nombre</b>
        </div>
        <div class="col">
        <b>Precio</b>
        </div>
        <div class="col">
        <b>Costo envío</b>
        </div>
        <div class="col">
        <b>Cantidad vendida</b>
        </div>
        <div class="col">
        <b>Modificar</b>
        </div>
        <div class="col">
        <b>Eliminar</b>
        </div>
    </div>
    <div class="row" v-for="instrumento in instrumentosData" :key="instrumento.id"  style="padding-top: 5px;">
        <div class="col">
        {{ instrumento.id }}
        </div>
        <div class="col">
        {{ instrumento.instrumento }}
        </div>
        <div class="col">
        {{ instrumento.precio }}
        </div>
        <div class="col">
        {{ instrumento.costoEnvio }}
        </div>
        <div class="col">
        {{ instrumento.cantidadVendida }}
        </div>
        <div class="col">
        <a class="btn btn-primary" :href="'/formulario/'+instrumento.id" role="button">Modificar</a>
        </div>
        <div class="col">
        <button type="button" @click="deleteInstrumento(instrumento.id)"  class="btn btn-danger" >Eliminar</button>
        </div>
    </div>
  </div>
</template>

<script lang="ts">
import Instrumento from '@/entitys/instrumento';
import { defineComponent, onMounted, ref } from 'vue';

export default defineComponent({
  name: 'GrillaInstrumentos',
  components: {},
  setup() { //setup se encargará de dispararse en la fase de inicialización del componente, al construir el componente, equivalente a created de V2
    const instrumentosData = ref<Instrumento[]>([]) //ref es el equivalente en react de useState, indico que la variable sera reactiva
    //V3
    onMounted(async () => {
                const res = await fetch(
                  "http://localhost:5000/api/instrumentos", {
                  method: 'GET',
                  headers: {
                    'Content-type': 'application/json',
                    'Access-Control-Allow-Origin':'*'
                  },
                  mode: 'cors'
                });
                const resJson = await res.json();
                console.log(resJson);
                instrumentosData.value = await resJson;
            });
    return {
      instrumentosData
    };
  },
  methods:{
    deleteInstrumento: async function (idInstrumento:number) {
        let urlServer = 'http://localhost:5000/api/instrumentos/delete/'+idInstrumento;
        console.log(urlServer);
        let result = await fetch(urlServer, {
            method: 'DELETE',
            headers: {
                'Content-type': 'application/json',
                'Access-Control-Allow-Origin':'*'
            },
            mode: 'cors'
	    });
        window.location.reload();

    }
    
  }
});
</script>

<style>

</style>