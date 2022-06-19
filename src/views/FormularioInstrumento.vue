<template>
  <div style="max-width: 75%; padding-left: 25%;">
  <div class="mb-3">
    <label for="exampleFormControlInput1" class="form-label">Nombre</label>
    <input v-model="instrumentoEncontrado.instrumento" type="text" class="form-control" id="exampleFormControlInput1" placeholder="Nombre" required>
  </div>
  <div class="mb-3">
    <label for="exampleFormControlInput3" class="form-label">Precio</label>
    <input v-model="instrumentoEncontrado.precio" type="number" class="form-control" id="exampleFormControlInput3" placeholder="Precio" required>
  </div>
  <div class="mb-3">
    <label for="exampleFormControlInput4" class="form-label">Imagen</label>
    <input v-model="instrumentoEncontrado.imagen" type="text" class="form-control" id="exampleFormControlInput4" placeholder="Imagen" required>
  </div>
  <div class="mb-3">
    <label for="exampleFormControlInput4" class="form-label">Costo envío</label>
    <input v-model="instrumentoEncontrado.costoEnvio" type="number" class="form-control" id="exampleFormControlInput4" placeholder="Costo envío" required>
  </div>
  <div class="mb-3">
    <label for="exampleFormControlInput4" class="form-label">Cantidad vendida</label>
    <input v-model="instrumentoEncontrado.cantidadVendida" type="number" class="form-control" id="exampleFormControlInput4" placeholder="Cantidad vendida" required>
  </div>
  <div class="col-auto">
    <button type="button" @click="guardar(instrumentoEncontrado)" class="btn btn-primary mb-3">Guardar</button>
  </div>
  </div>
</template>

<script lang="ts">
import Instrumento from '@/entitys/instrumento';
import router from '@/router';
import { defineComponent, onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

export default defineComponent({
  name: 'FormularioInstrumento',
  components: {},
  setup() {
    const route = useRoute()
    const instrumentoEncontrado = ref<Instrumento>(new Instrumento())
    onMounted(async () => {
              const parametroId:string = route.params.id as string;
              if(parseInt(parametroId) != 0){
                const res = await fetch(
                  "http://localhost:5000/api/instrumento/" + parametroId
                );
                const resJson = await res.json();
                instrumentoEncontrado.value = resJson;
              }
            });
    
    return {
      instrumentoEncontrado
    };
  },
  methods:{
    async guardar(instrumento:Instrumento) {
        let urlServer = 'http://localhost:5000/api/instrumentos/insert';
        let method = "POST";
        if(instrumento && instrumento.id > 0){
          urlServer = 'http://localhost:5000/api/instrumentos/update';
          method = "PUT";
        }
        await fetch(urlServer, {
          "method": method,
          "body": JSON.stringify(instrumento),
          "headers": {
          "Content-Type": 'application/json'
          }
        });
        router.push('/listaInstrumentos')
      }

    
  }
});
</script>

<style>

</style>