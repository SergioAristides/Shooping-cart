<template>
    <div class="main">
      <Loading v-if="loading">Loading...</Loading>
      <div v-else>
        <div class="container">
  <div class="product-card" v-for="(post, index) in posts" :key="post.id">
            <div class="product-info">
              <div class="image-container">
                <img :src="post.image" alt="product image" class="product-image">
              </div>
              <h2>{{ post.name }}</h2>
  
              <div class="two-product">
                <p>
                  <span class="price-icon">💲</span> {{ post.price }}
                </p>
                <p v-if="post.brand">&#8482;{{ post.brand }}</p>
              </div>
              <p v-if="post.expiration">{{ post.expiration }}</p>
              <div v-if="post.rating">
                <span class="stars" v-html="getRatingStars(post.rating)"></span>
              </div>
  
              <div class="botons">
                <button class="delete" @click="deleteProduct(post.id)">Eliminar</button>
                <button class="update" @click="modalUpdateProduct = true">Actualizar</button>
            </div>
            <button class="add">Agregar</button>
            </div>


            <div v-if="modalUpdateProduct" class="modal">
                <div class="modal-content">
                  <span class="close" @click="modalUpdateProduct = false">&#10060;</span>
                  <form method="PUT">
                    <div class="form-group">
                      <label for="name">Nombre:</label>
                      <input type="text" id="name" name="name" v-model="post.name">
                    </div>
                    <div class="form-group">
                      <label for="price">Precio:</label>
                      <input type="number" id="price" name="price" v-model="post.price">
                    </div>
                    <div class="form-group">
                      <label for="brand">Marca:</label>
                      <input type="text" id="brand" name="brand" v-model="post.brand">
                    </div>
                    <div v-if="post.expiration" class="form-group">
                      <label for="expiration">Fecha de vencimiento:</label>
                      <input type="date" id="expiration" name="expiration" v-model="post.expiration">
                    </div>
                    <div v-if="post.rating" class="form-group">
                      <label for="rating">Rating:</label>
                      <input type="number" id="rating" name="rating" v-model="post.rating">
                    </div>
                    <button type="submit" @click="poste(post)" >Actualizar</button>
                  </form>
                </div>
              </div>
          </div>
        </div>
      </div>
    </div>

  
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import Loading from './Loading.vue';
  const posts = ref([]);
  const loading = ref(true);
  const brands = ref([])
  const sizes = ref([])

  const modalUpdateProduct = ref(false);
const modalDeleteProduct = ref(false);
const modalAddProduct = ref(false);  
  
  const fetchData = async () => {
    loading.value = true;
    try {
      const response = await fetch("http://127.0.0.1:8000/products");
      const data = await response.json();
  
      data.forEach(product => {
        product.image = `https://picsum.photos/200/200?random=${product.id}`;
        product.count = 0; // Inicializa la propiedad count en 0 para cada producto
      });
  
      posts.value = data;
    } catch (error) {
      console.error(error);
    } finally {
      loading.value = false;
    }
  };
  

  fetchData();
  
  
  const getRatingStars = (rating) => {
    const roundedRating = Math.round(rating * 2) / 2; // Redondea al valor más cercano de media estrella
    let stars = ''; // Inicializa una cadena vacía para almacenar las estrellas
  
    // Crea una cadena de estrellas en función del rating redondeado
    for (let i = 0; i < 5; i++) {
      // Si el índice es menor que el rating redondeado, agrega una estrella completa
      if (i < Math.floor(roundedRating)) {
        stars += '&#9733;'; // Agrega una estrella completa
      } else {
        stars += '&#9734;'; // Agrega una estrella vacía
      }
    }
  
    return stars; // Devuelve la cadena de estrellas
  };
  
  const deleteProduct = async (id) => {
  try {
    const response = await fetch(`http://127.0.0.1:8000/products/${id}`, {
      method: 'DELETE',
    });
    if (response.ok) {
      // Si la eliminación es exitosa, actualiza los productos
      await fetchData();
    }
  } catch (error) {
    console.error(error);
  }
};


const updateProduct = async (post) => {
  const updatedData = {
    name: post.name,
    price: post.price,
    brand: post.brand,
    expiration: post.expiration,
    rating: post.rating,
    id : post.id
  };

  try {
    const response = await fetch(`http://127.0.0.1:8000/products/${id}`, {
      method: 'PUT',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(updatedData)
    });
    if (response.ok) {
      // Si la actualización es exitosa, actualiza los productos
      await fetchData();
      // Cierra la modal de actualización
      modalUpdateProduct.value = false;
    }
  } catch (error) {
    console.error(error);
  }
};


const poste = (post) =>{
  updateProduct(post);
  modalUpdateProduct=false;
}

  
  </script>
  
  <style scoped>
  
  .botons{
    display: flex;
    justify-content: center;
    text-align: center;
    padding: 0.5rem;
  }
  .botons button{
    width: 6rem;
    margin: 0.2rem;
    text-align: center;
    cursor: pointer;
    border-radius: 0.625rem;
  }
  .add{
    cursor: pointer;
    border-radius: 0.625rem;
  }

  .botons button:hover{
    transform: scale(1.1);
  }
  .add:hover{
    transform: scale(1.1);
    background-color: rgb(90, 219, 90);
    border-color: rgb(90, 219, 90)
  }
    .delete:hover{
        background-color: rgb(219, 90, 90);
        border-color: rgb(219, 90, 90);
    }
.update:hover{
    background-color: rgb(90, 90, 219);
    border-color: rgb(90, 90, 219);
    }
  .main {
    margin: 2rem;
    display: flex;
  }
  
  
  .container {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(12rem, 1fr));
    gap: 1.25rem;
  }
  
  .product-card:hover {
    -webkit-box-shadow: 6px 1px 87px 37px var(--blue-primary);
    -moz-box-shadow: 6px 1px 87px 37px var(--blue-primary);
    box-shadow: 6px 1px 87px 37px var(--blue-primary);
  }
  
  .product-card {
    border-radius: 0.625rem;
    background-color: white;
    box-shadow: 0 0.25rem 0.375rem rgba(0, 0, 0, 0.1);
    padding: 1.25rem;
  }
  
  
  .product-info {
    display: flex;
    flex-direction: column;
    text-align: center;
    
  }
  
  .product-info h2 {
    margin-top: 0;
  }
  
  .product-info p {
    margin-bottom: 0.3125rem;
  }
  
  .image-container {
    display: flex;
    justify-content: center;
  }
  
  .product-image {
    width: 10.5rem;
    height: 10.5rem;
    border-radius: 35%;
    object-fit: cover;
    margin-right: 20px;
  }
  
  .price-icon {
    font-size: 1.25rem;
    margin-right: 0.3125rem;
  }
  
  .two-product {
    display: flex;
    justify-content: space-between;
  }
  /*stars*/
  .stars {
    font-size: 1.25rem;
    color: #f39c12;
  }
  
  .pointer {
    cursor: pointer;
  }
  
  .bto {
    background-color: var(--blue-primary);
    border-color: var(--blue-primary);
  }
  .bto:hover {
    transform: scale(1.1);
    background-color: rgb(90, 219, 90);
    border-color: rgb(90, 219, 90);
  }
  /**/

  /* Estilos para la modal */
.modal {
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    -webkit-box-shadow: 6px 1px 87px 37px var(--blue-primary);
    -moz-box-shadow: 6px 1px 87px 37px var(--blue-primary);
    box-shadow: 6px 1px 87px 37px var(--blue-primary);
    border-radius: 1rem 1rem 1rem 1rem;

  }
  
  .modal-content {
    background-color: #fefefe;
    padding: 20px;
    border: 1px solid #888;
    max-width: 400px; /* Ancho máximo de la modal */
  }
  
  .close {
    color: #aaa;
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 24px;
    cursor: pointer;
  }
  
  .close:hover {
    color: black;
  }
  </style>