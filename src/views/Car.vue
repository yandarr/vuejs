<template>
  <div class="container">
    <div class="card mt-4">
      <div class="card-header">
        Productos en el carrito
      </div>
      <div class="card-body">
        <template v-if="products.length === 0">
          <p>No existen productos</p>
        </template>
        <template v-if="products.length > 0">
          <table class="table table-hover table-borderless">
            <thead>
              <tr>
                <th scope="col"></th>
                <th scope="col">Name</th>
                <th scope="col">Price</th>
                <th scope="col">Inventory</th>
                <th scope="col">Quantity</th>
                <th scope="col"></th>
              </tr>
            </thead>
            <tbody>
              <template v-for="product in products" :key="product.id">
                  <ProductCar :product="product" :reloadCarFun="reloadCarFun" />
              </template>
            </tbody>
          </table>
          <label class="labelemail" for="email">Por favor, ingrese un email para continuar con el pedido</label>
          <input type="email" 
            v-model="email" 
            placeholder="juan.rua@est.iudigital.edu.co" 
            class="form-control" />
          <button @click="buyNow" class="btn btn-warning btn-fill">COMPRAR AHORA</button>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import { get, removeAll } from '../services/car';
import { ref, watchEffect } from 'vue';
import ProductCar from '../components/ProductCar.vue';
import Swal from 'sweetalert2';
import { createOrder } from '../services/order';

export default {
  components: {
    ProductCar
  },
  setup() {
    const products = ref([]);
    const reloadCar = ref(false);
    const email = ref('');

    watchEffect(() => {
      
      reloadCar.value;

      products.value = get();

    });

    const reloadCarFun = () => {
      reloadCar.value = !reloadCar.value;
    }

    const buyNow = async () => {

      if (email.value === '') {
        Swal.fire(
        'Información',
        'Por favor escriba un email',
        'info'
        );

      } else {
        for (const product of products.value) {
          
          if (!product.quantity || product.quantity <= 0) {
            
            Swal.fire(
            'Información',
            `Selecciona una cantidad para ${product.name}`,
            'info'
            );
            return;
          }
            
        }

        const order = {
          email: email.value,
          products: products.value,
        }

        Swal.fire({
          allowOutsideClick: false,
          text: 'Cargando...'
        });
        
        Swal.showLoading();

        const resp = await createOrder(order);

        if (!resp.ok) {
          console.log('Error al guardar');
        } else {
          console.log('OK')
          removeAll();
          products.value = [];
          email.value = '';
        }
        Swal.close();

      }

    }

    return {
      products,
      reloadCarFun,
      buyNow,
      email,
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
    background-color: #b20cc4;
    margin: 10px 0 10px 0;
  }

  .btn:hover {
    background-color: #90099f;
  }
  .btn:active {
      background-color: #90099f;
  }
  .labelemail{
    margin: 10px 0;
    color: #656565;
  }
</style>