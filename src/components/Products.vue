<template>
  <div class="main">
    <div class="accordion acc" id="accordionExample" >
      <div class="accordion-item">
        <h2 class="accordion-header" id="headingOne">
          <button class="accordion-button" type="button" data-bs-toggle="collapse" data-bs-target="#collapseOne" aria-expanded="true" aria-controls="collapseOne">
            Marca
          </button>
        </h2>
        <div id="collapseOne" class="accordion-collapse collapse show" aria-labelledby="headingOne" data-bs-parent="#accordionExample">
          <div class="accordion-body">
            <div class="form-check" v-for="(brand, index) in brands" :key="index">
              <input class="form-check-input" type="checkbox" :id="'brand_' + index" :value="brand" v-model="selectedBrands">
              <label class="form-check-label" :for="'brand_' + index">{{ brand }}</label>
            </div>
          </div>
        </div>
        
        
      </div>
<div class="accordion-item">
  <h2 class="accordion-header" id="headingTwo">
    <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#collapseTwo" aria-expanded="false" aria-controls="collapseTwo">
      talla
    </button>
  </h2>
  <div id="collapseTwo" class="accordion-collapse collapse show" aria-labelledby="headingOne" data-bs-parent="#accordionExample">
    <div class="accordion-body">
      <div class="form-check" v-for="(size, index) in sizes" :key="index">
        <input class="form-check-input" type="checkbox" :id="'size_' + index" :value="size" v-model="selectedSizes">
        <label class="form-check-label" :for="'size_' + index">{{ size }}</label>
      </div>
    </div>
  </div>
</div>
      <div class="accordion-item">
        <h2 class="accordion-header" id="headingThree">
          <button class="accordion-button collapsed" type="button" data-bs-toggle="collapse" data-bs-target="#collapseThree" aria-expanded="false" aria-controls="collapseThree">
            Price
          </button>
        </h2>
        <div id="collapseThree" class="accordion-collapse collapse" aria-labelledby="headingThree" data-bs-parent="#accordionExample">
          <div class="accordion-body">
            <div class="form-check">
              <input class="form-check-input" type="radio" id="priceRange1" name="priceRange" value="20000-50000" v-model="selectedPriceRange" >
              <label class="form-check-label" for="priceRange1">$20,000 - $50,000</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="radio" id="priceRange2" name="priceRange" value="50000-100000" v-model="selectedPriceRange">
              <label class="form-check-label" for="priceRange2">$50,000 - $100,000</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="radio" id="priceRange2" name="priceRange" value="100000-200000" v-model="selectedPriceRange">
              <label class="form-check-label" for="priceRange2">$100,000 - $200,000</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="radio" id="priceRange2" name="priceRange" value="200000-500000" v-model="selectedPriceRange">
              <label class="form-check-label" for="priceRange2">$200,000 - $500,000</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="radio" id="priceRange2" name="priceRange" value="500000-1000000" v-model="selectedPriceRange">
              <label class="form-check-label" for="priceRange2">$500,000 - $100,000</label>
            </div>
          </div>
        </div>
      </div>
    </div>

    <Loading v-if="loading">Loading...</Loading>
    <div v-else>
      <div class="container">
        <div class="product-card" v-for="(post, index) in (filteredPosts.length > 0 ? filteredPosts : posts)" :key="post.id">
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

            <div class="d-flex justify-content-evenly mb-2 mt-2 cursor-pointer">
              <span class="rounded bg-body-secondary w-25 pointer" @click="menos(index)">&#45;</span>
              <p>{{post.count}}</p>
              <span class="rounded bg-body-secondary w-25 pointer" @click="mas(index)">&#43;</span>
            </div>
            <button class="btn btn-primary bto" @click="addToCart(index) ">Add to cart</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Loading from './Loading.vue';
import { ref , computed,defineEmits} from 'vue';
import { store } from './helpers/store.js';

const posts = ref([]);
const loading = ref(true);
const brands = ref([])
const sizes = ref([])
const selectedBrands = ref([]);
const selectedSizes = ref([]);
const selectedPriceRange = ref(null);
const count = ref(0);
const shoppingCart = store.shoppingCart;
const emit = defineEmits(['resetCounts']); // Definir el evento para reiniciar los conteos


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
    const uniqueBrands = [...new Set(data.map(product => product.brand))];
    brands.value = uniqueBrands.filter(brand => !!brand); // 

    const uniqueSizes = [...new Set(data.map(product => product.size))];
    sizes.value = uniqueSizes.filter(size => !!size); //

    console.log(sizes.value);
    console.log(brands.value);
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



const filteredPosts = computed(() => {
  let filtered = posts.value;


  if (selectedBrands.value.length) {
    filtered = filtered.filter(post => selectedBrands.value.includes(post.brand));
  }

  if (selectedSizes.value.length) {
    filtered = filtered.filter(post => selectedSizes.value.includes(post.size));
  }

  if (selectedPriceRange.value) {
    const [min, max] = selectedPriceRange.value.split('-').map(Number);
    filtered = filtered.filter(post => post.price >= min && post.price <= max);
  }

  return filtered;
});


const menos = (index) => {
  if (posts.value[index].count !== undefined && posts.value[index].count > 0) {
    posts.value[index].count--;
  }
}

const mas = (index) => {
  if (posts.value[index].count !== undefined) {
    posts.value[index].count++;
  }
}

const addToCart = (index) => {
  if (verificar(index)) {
    // Verifica que shoppingCart no sea undefined antes de usar push
    if (shoppingCart) {
      shoppingCart.push(posts.value[index]);      
    } else {
      console.error("El carrito de compras es indefinido");
    }
  } else {
    alert("¿Cuántos deseas comprar?");
  }

}

/* metodo para descartar producto con count =0 y sacar advertencia que diga cuantos deseas comprar */

const verificar = (index) => {
  if (posts.value[index].count === 0) {
    return false;
  }else{
    return true;
  }

}


const reiniciar = () => {
  posts.value.forEach(post => {
    post.count = 0;
  });
  emit('resetCounts'); // Emitir el evento para reiniciar los conteos
}

</script>

<style scoped>


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
</style>