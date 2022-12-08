<template>
  <div class="container">
    <div class="card mt-4">
      <div class="card-header">
        Vendedores
      </div>
      <div class="card-body">
        <input type="text" 
          v-model="name_seller" 
          placeholder="Escriba nombre del Seller"
          class="form-control"/>
        <button @click="CreateS" class="btn btn-warning btn-fill">Crear</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, watchEffect } from 'vue';
import Seller from '../components/Seller.vue';
import Swal from 'sweetalert2';
import { createSeller } from '../services/seller';

export default {
  components: {
    Seller
  },
  setup() {
    const name_seller = ref('');

    const CreateS = async () => {
      
      if (name_seller.value === '') {
        Swal.fire(
        'Información',
        'Por favor escriba el nombre del Seller',
        'info'
        );
        //return;
      } else {
        const seller = {
          name: name_seller.value,
        }

        Swal.fire({
          allowOutsideClick: false,
          text: 'Loading...'
        });
        
        Swal.showLoading();
        console.log(seller);
        const resp = await createSeller(seller);

        if (!resp.ok) {
          console.log('Error al guardar');
        } else {
          console.log('OK')
          name_seller.value = '';
        }
        Swal.close();

      }
    }
    return {

      CreateS,
      name_seller,
    }
  }
}
</script>

<style scoped>
.card-header {
    background: #b20cc4;
    color: white;
    font-size: 16px;
    text-align: center;
  }
  .btn{
    margin: 10px 0 10px 0;
    background-color: #b20cc4;
    color: white;
  }
</style>