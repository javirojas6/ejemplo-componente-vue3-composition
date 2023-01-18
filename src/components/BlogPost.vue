<script setup>

//1const props = defineProps(['title', 'id', 'body', 'colorText'])

defineProps ({
  title: String,
  id: Number,
  body: {
    type: String,
    default: 'sin descripción'
  },
  colorText: String,
  cambiarFavorito: Function,
})

const emit = defineEmits(['cambiarFavoritoNombre'])

//emit('cambiarFavoritoNombre',title)

</script>



<template>
  <div class="card">
    <div class="card-body">
        <h5 
        class="card-title"
        :class="`text-${colorText}`"
        >{{ id }} - {{ title }}</h5>
        <p>{{ body }}</p>
        <button @click="cambiarFavorito(title)"
                class="btn btn-outline-primary">
                Favorito
        </button>
        <!-- <button @click="emit('cambiarFavoritoNombre',title)" manera más correcta para emisiones de evento
                class="btn btn-outline-primary">
                Favorito
        </button> -->
    </div>
  </div>
</template>