<script setup>

const joke = ref(null);
const loading = ref(false);
const error = ref(null);

const fetchJoke = async () => {
loading.value = true;
error.value = null;

try {
    joke.value = await $fetch('https://icanhazdadjoke.com/', {
    headers: { 'Accept': 'application/json' }
    });
} catch (e) {
    error.value = 'Errore nel recupero dell\'aneddoto.';
} finally {
    loading.value = false;
}
};
</script>


<template>
    <div>
        <h1>Aneddoti Divertenti</h1>
        <button @click="fetchJoke" :disabled="loading">Ottieni un aneddoto</button>
        <div v-if="loading">Caricamento...</div>
        <div v-if="error">Errore: {{ error }}</div>
        <div v-if="joke">
            <p>{{ joke.joke }}</p>
        </div>
    </div>
</template>


