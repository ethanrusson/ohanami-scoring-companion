<script setup>
import { ref } from 'vue'
const emit = defineEmits(['players']);
let currentPlayers = ref([]);

function addPlayer() {
    currentPlayers.value.push(
        {
            id: currentPlayers.value.length,
            name: '',
            scores: {
                'water': [0,0,0],
                'plants': [0,0,0],
                'stone': [0,0,0],
                'blossoms': [0,0,0]
            }
        });
}
function removePlayer(id) {
    for(let i = 0; i < currentPlayers.value.length; i++) {
        if (currentPlayers.value[i].id === id) {
            currentPlayers.value.splice(i, 1);
        }
    }
}
function startScoring() {
    emit('players', currentPlayers.value);
}
</script>

<template>
    <div id="add-players-page">
        <div>
            <h1>Players</h1>
            <div v-for="player of currentPlayers" :key="player.id" class="player-card">
                <input v-model="player.name" placeholder="Enter player name" />
                <button tabindex="-1" class="button-secondary" @click='removePlayer(player.id)'>
                    <img src="../assets/close.svg" />
                </button>
            </div>
            <button
                class="button-secondary"
                @click="addPlayer()"
                :disabled="currentPlayers.length > 7"
            >
                <img src="../assets/add.svg" /> Add Player
            </button>
        </div>
        <button class="button-primary" @click='startScoring()'>Start Scoring</button>
    </div>
</template>

<style scoped lang="scss">
#add-players-page {
    margin-top: 24px;
    padding-bottom: 24px;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    h1 {
        margin-bottom: 12px;
    }
    button {
        width: 100%;
    }
    .player-card {
        display: flex;
        align-items: center;
        margin-bottom: 12px;
        input {
            border: none;
            height: 48px;
            padding: 0px 12px;
            border-radius: 12px;
            flex: 1;
        }
        button {
            width: fit-content;
            background: none !important;
            &::before {
                width: calc(100% - 4px);
                height: calc(100% - 4px);
                top: 0px;
                left: 0px;
            }
        }
    }
}
</style>
