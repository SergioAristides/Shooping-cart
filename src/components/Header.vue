<template>
    <header>
        <nav class="header">
            <div class="icon">
                <img src="../images/icon2r.png" alt="shopping cart icon"> 
                <img src="../images/shoppingCart.png" alt="shopping cart icon"> 
            </div>   
            <div class="menu-hamburguer" @click="toggleModal">
                <p>&#9776; Menú</p>
            </div>
            <div class="search">
                <input type="text" placeholder="Buscar">
            </div>
            <div class="shopping-cart">
                <span class="cart" @click="handleClick" >&#128722;</span>
            </div>
        </nav>
    
        <div class="modal" v-if="modalShow">
            <div class="modal-content">
                <span class="close" @click="toggleModal">&#10060;</span>
                <div class="icon-modal">
                    <img src="../images/shoppingCarte.png" alt="shopping cart icon">
                </div>   
                <ul>
                    <li>Inicio</li>
                    <li>Productos</li>
                    <li>Contacto</li>
                </ul>
            </div>
        </div>
    
            <!-- Modal del carrito -->
    <div class="modal" v-if="cartModalShow">
        <div class="modal-content">
          <span class="close" @click="closeCartModal">&#10060;</span>
          <img src="../images/shoppingCarte.png" alt="">
          <ul>
            <li v-for="product in shoppingCart" :key="product.id">
              {{ product.name }} - Cantidad: {{ product.count }}
            </li>
          </ul>
            <p>Total a pagar: ${{ totalApagar }}</p>
          <button @click="checkout">Pagar</button>
        </div>
      </div>

    </header>
   
</template>

<script setup>
import { ref,computed, defineEmits} from 'vue';
import { store} from './helpers/store.js';

const modalShow = ref(false);
const cartModalShow = ref(false);
const emit = defineEmits(['myEvent']);

const shoppingCart = store.shoppingCart;

const toggleModal = () => {
    modalShow.value = !modalShow.value;
}

const showCartModal = () => {
  cartModalShow.value = true;
}

const closeCartModal = () => {
  cartModalShow.value = false;
}

const totalApagar = computed(() => {
  return shoppingCart.reduce((acc, product) => {
    return acc + product.price * product.count;
  }, 0);
});

const handleClick = () => {
    showCartModal(); // Llamas al primer método
    emit('myEvent'); // Emite el evento personalizado
}
</script>



<style scoped>

.header{
    margin-bottom: 3rem;
    position: fixed;
    width: 100%;
    z-index: 100;
}
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.5); /* Fondo semi-transparente */
}
.modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    -webkit-box-shadow: 17px 10px 46px 19px #3f3e3e;
    -moz-box-shadow: 17px 10px 46px 19px #3f3e3e;
    box-shadow: 17px 10px 46px 19px #3f3e3e;
    width: 20rem;
}

.modal-content ul {
    list-style: none;
    padding: 0;
}
/*hover para los li*/
.modal-content ul li:hover {
    background-color: #f8f8f8;
    cursor: pointer;
    border-left : 0.5rem solid #9fcdd3;
    border-radius: 0.3rem;
}
.modal-content ul li {
    margin-bottom: 10px;
}

/* Estilos para el botón de cierre */
.close {
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
}

    nav {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background-color: #f8f8f8;
        padding: 10px 20px;
        -webkit-box-shadow: 1px 25px 19px -9px #8E8E8E;
        -moz-box-shadow: 1px 25px 19px -9px #8E8E8E;
        box-shadow: 1px 25px 19px -9px #8E8E8E;
    }

    .icon img {
        width: 6.5rem;
        border-radius: 0.5rem;
 
    }


    .search input {
        width: 100%;
        padding: 5px;
        border-radius: 5px;
        border: 1px solid #ccc;
    }
    .menu-hamburguer p {
        font-size: 1.8rem;
        font-weight: bold;
        color : #8E8E8E;
        cursor: pointer;

    }

    .shopping-cart {
        font-size: 3rem;
        color: #8E8E8E;
    }

    .icon-modal {
        display: flex;
        justify-content: center;
        margin-bottom: 1rem;
        margin-top: 1.5rem;
    }
    .icon-modal img {
        width: 9rem;
        border-radius: 0.5rem;
    }

    .cart{
        cursor: pointer;
    }
</style>