<template>
    <div class="container">
        <h1>Babbo Natale Segreto</h1>

        <form @submit.prevent="addParticipant">
            <input v-model="newParticipant" type="text" placeholder="Inserisci il nome" required />
            <button type="submit">Aggiungi Partecipante</button>
        </form>

        <ul>
            <li v-for="(participant, index) in participants" :key="index">
                {{ participant }}
                <button @click="removeParticipant(index)">Rimuovi</button>
            </li>
        </ul>

        <button @click="generateSecretSanta" :disabled="participants.length < 2">Genera Babbo Natale Segreto</button>

        <div v-if="assignments.length > 0">
            <h2>Risultati</h2>
            <ul>
                <li v-for="(pair, index) in assignments" :key="index">
                    {{ pair.giver }} → {{ pair.receiver }}
                </li>
            </ul>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const newParticipant = ref('');
const participants = ref([]);
const assignments = ref([]);

const addParticipant = () => {
    if (newParticipant.value && !participants.value.includes(newParticipant.value)) {
        participants.value.push(newParticipant.value);
        newParticipant.value = '';
    }
};

const removeParticipant = (index) => {
    participants.value.splice(index, 1);
};

const generateSecretSanta = () => {
    const shuffled = [...participants.value].sort(() => Math.random() - 0.5);

    assignments.value = shuffled.map((giver, index) => {
        const receiver = shuffled[(index + 1) % shuffled.length];
        return { giver, receiver };
    });
};
</script>

<style scoped>
.container {
    max-width: 600px;
    margin: 0 auto;
    text-align: center;
}

form {
    margin-bottom: 1rem;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    margin-bottom: 0.5rem;
}

button {
    margin-left: 1rem;
}
</style>
