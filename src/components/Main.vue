<template>
   
        <Header></Header>

        <main>
            
            <div class="main-image">
                <img src="../images/imageMain.jpeg" alt="Main image">
            </div> 
            <div class="admin">
                <h2>Admin</h2>
                <p>¡Bienvenido!</p>
                <button class="btn btn-primary" @click="toggleModal">get in</button>
            </div>
            
                <div v-if="modalShow" class="modale">
                    <LoginModal @myEvent="toggleModal" />
                </div>

        </main>

        <ProductsAdmin></ProductsAdmin>

      
</template>

<script setup>

import { ref,reactive } from 'vue';
import LoginModal from './LoginModal.vue';
import Header from './Header.vue';
import Footer from './Footer.vue';
import Products from './Products.vue';
import ProductsAdmin from './ProductsAdmin.vue';

const modalShow = ref(false);

const toggleModal = () => {
    modalShow.value = !modalShow.value;
}
</script>

<style script>


  
  /* Estilos para el footer */


/*main*/

main{
    display: flex;
    justify-content: space-around;
    align-items: center;
    text-align: center;
}
/*main*/

/*boton admin*/
.admin button{
    background-color: var(--blue-primary);
    border-color: var(--blue-primary);
}

.admin button:hover{
    transform: scale(1.1);
}

/*boton admin*/

/*main image*/

.main-image img{
    width: 30rem;
    border: 2rem var(--blue-primary);
    border-radius: 1rem; 
    -webkit-box-shadow: 6px 1px 87px 37px rgba(0, 0, 0, 0.5);
    -moz-box-shadow: 6px 1px 87px 37px rgba(0, 0, 0, 0.5);
    box-shadow: 6px 1px 87px 37px rgba(0, 0, 0, 0.5);
    margin-top: 10rem;

}

/*main image*/


/*Modal*/
.modale{
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
/*Modal*/
</style>style