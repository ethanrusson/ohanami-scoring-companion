<script setup>
import { ref } from 'vue'

defineProps({
    players: Array
})

const emit = defineEmits(['returnToScoring']);

function returnToScoring() {
    emit('returnToScoring', true);
}
</script>

<template>
    <div id="calculate-scores-page">
        <button id="back-to-scoring" @click="returnToScoring()">
            <img src="../assets/arrow-back.svg" />Back to Scoring
        </button>
        <div id="players-container">
            <div v-for="(player, index) of players" :key="player.id" class="player-card">
                <div class="player-card-header">
                    <p>
                        <span>{{ index + 1 }}</span>
                        {{ player.name }}
                    </p>
                    <span>Total: {{ player.totalScore }}</span>
                </div>
                <div class="score-details">
                    <span class="water">
                        <img src="../assets/water.svg" />
                        {{ player.scores.water[3] }} points
                    </span>
                    <span class="plants">
                        <img src="../assets/plants.svg" />
                        {{ player.scores.plants[3] }} points
                    </span>
                    <span class="stone">
                        <img src="../assets/stone.svg" />
                        {{ player.scores.stone[3] }} points
                    </span>
                    <span class="blossoms">
                        <img src="../assets/blossoms.svg" />
                        {{ player.scores.blossoms[3] }} points
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
$c_primary: #b15b55;
$c_water: #577abd;
$c_plants: #7cc5a6;
$c_stone: #c0c5c2;
$c_blossoms: #ecc3c3;
#calculate-scores-page {
    margin-top: 24px;
    flex: 1;
    display: flex;
    flex-direction: column;
    #back-to-scoring {
        background: none;
        padding: 0px;
        text-align: left;
        width: fit-content;
        margin-bottom: 12px;
        img {
            margin-right: 12px;
        }
    }
    #players-container {
        max-height: calc(100vh - 240px);
        overflow-y: scroll;
        &::-webkit-scrollbar {
            width: 6px;
        }

        &::-webkit-scrollbar-thumb {
            background-color: white;
            border-radius: 6px;
            border: 6px solid rgba(255, 255, 255, 0);
        }
        .player-card {
            height: 100%;
            min-width: 300px;
            padding: 12px;
            margin-right: 12px;
            margin-bottom: 12px;
            background: rgba(white, 0.8);
            border-radius: 12px;
            .player-card-header {
                display: flex;
                align-items: center;
                justify-content: space-between;
                margin: 0px 12px 12px 0px;
                span {
                    color: $c_primary;
                    font-weight: bold;
                }
                p {
                    color: darken($c_primary, 20%);
                }
            }
            .score-details {
                display: flex;
                span {
                    flex: 1;
                    max-width: 175px;
                    display: inline-flex;
                    align-items: center;
                    margin-right: 12px;
                    padding: 6px;
                    border-radius: 12px;
                    &.water {
                        background: rgba($c_water, 0.12);
                        input {
                            color: darken($c_water, 10%);
                            &:focus {
                                outline: 2px solid $c_water;
                            }
                        }
                        button {
                            background: $c_water !important;
                        }
                    }
                    &.plants {
                        background: rgba($c_plants, 0.24);
                        input {
                            color: darken($c_plants, 20%);
                            &:focus {
                                outline: 2px solid $c_plants;
                            }
                        }
                        button {
                            background: $c_plants;
                        }
                    }
                    &.stone {
                        background: rgba($c_stone, 0.24);
                        input {
                            color: darken($c_stone, 20%);
                            &:focus {
                                outline: 2px solid $c_stone;
                            }
                        }
                        button {
                            background: $c_stone;
                        }
                    }
                    &.blossoms {
                        background: rgba($c_blossoms, 0.24);
                        input {
                            color: darken($c_blossoms, 20%);
                            &:focus {
                                outline: 2px solid $c_blossoms;
                            }
                        }
                        button {
                            background: $c_blossoms;
                        }
                    }
                    &:last-of-type {
                        margin-right: 0px;
                    }
                }
            }
        }
    }
}
</style>
