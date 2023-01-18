
<script setup>
import ButtonCounter from "./components/ButtonCounter.vue";
import BlogPost from "./components/BlogPost.vue";
import {ref,computed, onMounted} from "vue";
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue"

// const posts = ref ([

//   {title: 'Post 1', id: 1,body: 'descripcion 1'},
//   {title: 'Post 2', id: 2,body: 'descripcion 2'},
//   {title: 'Post 3', id: 3},
// ])
const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const favorito = ref ("");
const loading = ref(false);

const cambiarFavorito = (title) => {
  favorito.value = title;
};

const next = () => {
  inicio.value = inicio.value + postXpage;
  fin.value = fin.value + postXpage;
}

const prev = () => {
  inicio.value = inicio.value - postXpage;
  fin.value = fin.value - postXpage;
}

onMounted(async() => {
  loading.value = true;
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
  }catch(error){
    console.log(error)
  }finally {
    setTimeout(() => {
       loading.value = false
     },1000);
  }

})

// fetch('https://jsonplaceholder.typicode.com/posts')
//   .then((res) => res.json())
//   .then((data) => {
//     posts.value = data;
//   })
//   .finally(() => {
//     setTimeout(() => {
//       loading.value = false
//     },1000);
    
    
//   });


  const maxLength = computed (()=> posts.value.length)


</script>

<template>
  <LoadingSpinner v-if="loading"/>

  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mi post favorito: {{ favorito }}</h2>
    
    <PaginatePost
     @next="next"
     @prev="prev"
     :inicio="inicio"
     :fin="fin"
     :maxlength="maxLength"
     :max="posts.length"
     class="mb-2">
      
    </PaginatePost>

    <!-- <ButtonCounter></ButtonCounter> -->
  
    <BlogPost
      v-for="post in posts.slice(inicio,fin)"
      :key="post.id"
      :title="post.title"
      :id="post.id" 
      :body="post.body" 
      :cambiarFavorito="cambiarFavorito"
      class="mb-2"
    ></BlogPost>
    <!--@cambiarFavoritoNombre="cambiarFavorito"-->
    <!-- <BlogPost
      title="Post 2" :id="2" body="descripcion 2" colorText="secondary"
    />
    <BlogPost
      title="Post 3" :id="3" colorText="success"
    /> -->
  </div>
</template>