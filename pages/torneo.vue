<template>
    <div>
        <h1>Torneo a eliminazione diretta</h1>

        <!-- Aggiungi giocatori -->
        <form @submit.prevent="addPlayer">
            <input v-model="newPlayer" placeholder="Aggiungi un giocatore" />
            <button type="submit">Aggiungi</button>
        </form>

        <h2>Giocatori</h2>
        <ul>
            <li v-for="player in players" :key="player">{{ player }}</li>
        </ul>

        <!-- Inizia torneo -->
        <button @click="startTournament" :disabled="players.length < 2">Inizia Torneo</button>

        <!-- Bracket -->
        <div v-if="currentRound.length && !winner">
            <h2>Bracket del Torneo - Turno {{ currentRoundNumber }}</h2>
            <ul>
                <li v-for="(match, index) in currentRound" :key="index">
                    <span v-if="match.completed">
                        {{ match.player1 }} vs {{ match.player2 }} è stata vinta da {{ match.winner }}
                    </span>
                    <span v-else>
                        {{ match.player1 }} vs {{ match.player2 }}
                        <span v-if="match.player2 === 'RIPOSA'">{{ match.player1 }} RIPOSA</span>
                        <button v-if="match.player2 !== 'RIPOSA'"
                            @click="advanceWinner(match.player1, match.player2, index)">
                            Vince {{ match.player1 }}
                        </button>
                        <button v-if="match.player2 !== 'RIPOSA'"
                            @click="advanceWinner(match.player2, match.player1, index)">
                            Vince {{ match.player2 }}
                        </button>
                    </span>
                </li>
            </ul>
        </div>

        <!-- Squadra che riposa -->
        <div v-if="restingTeam">
            <h3>{{ restingTeam }} RIPOSA questo turno.</h3>
        </div>

        <!-- Squadra ripescata -->
        <div v-if="rescuedTeam">
            <h3>{{ rescuedTeam }} è stata ripescata per giocare questo turno!</h3>
        </div>

        <!-- Vincitore -->
        <div v-if="winner">
            <h2>Vincitore del Torneo: {{ winner }}</h2>
        </div>
    </div>
</template>



<script setup>
import { ref } from 'vue'

const players = ref([]) // Lista di giocatori
const newPlayer = ref('') // Nuovo giocatore da aggiungere
const currentRound = ref([]) // Bracket del round corrente
const winner = ref(null) // Vincitore del torneo
const currentRoundNumber = ref(1) // Numero del round corrente
let nextRoundPlayers = [] // Giocatori che avanzano al prossimo round
let eliminatedPlayers = [] // Giocatori eliminati nei round precedenti
const restingTeam = ref(null) // Squadra che riposa
const rescuedTeam = ref(null) // Squadra ripescata

// Aggiungi un giocatore alla lista
const addPlayer = () => {
    if (newPlayer.value.trim()) {
        players.value.push(newPlayer.value)
        newPlayer.value = ''
    }
}

// Crea i bracket del primo turno
const startTournament = () => {
    winner.value = null
    currentRoundNumber.value = 1
    eliminatedPlayers = []
    nextRoundPlayers = [...players.value].sort(() => 0.5 - Math.random()) // Mischia i giocatori
    restingTeam.value = null
    rescuedTeam.value = null
    createRound()
}

// Crea un turno a partire dai giocatori rimasti
const createRound = () => {
    currentRound.value = []
    restingTeam.value = null
    rescuedTeam.value = null

    for (let i = 0; i < nextRoundPlayers.length; i += 2) {
        const player1 = nextRoundPlayers[i]
        const player2 = nextRoundPlayers[i + 1] || 'RIPOSA' // Se dispari, il giocatore riposa
        if (player2 === 'RIPOSA') {
            restingTeam.value = player1 // Memorizza la squadra che riposa
        }
        currentRound.value.push({ player1, player2, completed: false, winner: null })
    }

    nextRoundPlayers = [] // Resetta i giocatori che avanzano al prossimo round

    // Avanzamento automatico del giocatore che riposa
    checkForRestingPlayers()
}

// Verifica se un giocatore riposa e avanzalo automaticamente
const checkForRestingPlayers = () => {
    currentRound.value.forEach((match, index) => {
        if (match.player2 === 'RIPOSA') {
            match.completed = true
            match.winner = match.player1 // Il giocatore riposa, quindi avanza
            nextRoundPlayers.push(match.player1)
        }
    })

    if (currentRound.value.every(match => match.completed)) {
        // Se tutte le partite del turno sono completate, avanza al turno successivo
        currentRoundNumber.value++
        if (nextRoundPlayers.length === 1) {
            winner.value = nextRoundPlayers[0] // Se rimane solo un giocatore, è il vincitore
        } else {
            handleOddPlayers() // Gestisci giocatori dispari al turno successivo
            createRound()
        }
    }
}

// Gestisci i giocatori dispari ripescando un eliminato
const handleOddPlayers = () => {
    if (nextRoundPlayers.length % 2 !== 0 && currentRoundNumber.value > 1) {
        if (eliminatedPlayers.length > 0) {
            // Ripescaggio di un eliminato casualmente
            const chosenEliminatedPlayer = eliminatedPlayers[Math.floor(Math.random() * eliminatedPlayers.length)]
            nextRoundPlayers.push(chosenEliminatedPlayer)

            // Memorizza la squadra ripescata (aggiorna reattivamente)
            rescuedTeam.value = chosenEliminatedPlayer
            
            // Log per verificare il ripescaggio
            console.log('Squadra ripescata:', rescuedTeam.value)

            // Rimuovi il giocatore ripescato dalla lista degli eliminati
            eliminatedPlayers = eliminatedPlayers.filter(player => player !== chosenEliminatedPlayer)
        }
    }
}

// Avanza il vincitore al prossimo turno e salva i giocatori eliminati
const advanceWinner = (winningPlayer, losingPlayer, matchIndex) => {
    nextRoundPlayers.push(winningPlayer)

    // Aggiorna lo stato del match come completato
    currentRound.value[matchIndex].completed = true
    currentRound.value[matchIndex].winner = winningPlayer

    if (losingPlayer !== 'RIPOSA') {
        eliminatedPlayers.push(losingPlayer) // Aggiungi l'eliminato alla lista dei ripescabili
    }

    // Se tutte le partite del turno sono state giocate
    if (currentRound.value.every(match => match.completed)) {
        if (nextRoundPlayers.length === 1) {
            // Se rimane un solo giocatore, è il vincitore del torneo
            winner.value = nextRoundPlayers[0]
        } else {
            // Altrimenti, crea il turno successivo
            currentRoundNumber.value++
            handleOddPlayers() // Gestisci eventuali dispari nel turno successivo
            createRound()
        }
    }
}
</script>
