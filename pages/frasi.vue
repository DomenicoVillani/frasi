<script setup>

const quote = ref(null);
const loading = ref(false);
const error = ref(null);

const fetchQuote = async () => {
loading.value = true;
error.value = null;

try {
    quote.value = await $fetch('https://api.quotable.io/random');
} catch (e) {
    error.value = 'Errore nel recupero della citazione.';
} finally {
    loading.value = false;
}
};
</script>




<template>
    <div>
        <h1>Citazioni Motivazionali</h1>
        <button @click="fetchQuote" :disabled="loading">Ottieni una citazione</button>
        <div v-if="loading">Caricamento...</div>
        <div v-if="error">Errore: {{ error }}</div>
        <div v-if="quote">
            <p>"{{ quote.content }}"</p>
            <p>- {{ quote.author }}</p>
        </div>
    </div>
</template>





