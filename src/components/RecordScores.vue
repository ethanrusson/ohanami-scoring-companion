<script setup>
import { ref } from 'vue'

defineProps({
    players: Array
})

const emit = defineEmits(['calculateScores']);
let selectedRound = ref(1);

function selectRound(number) {
    selectedRound.value = number;
}

function calculateScores() {
    emit('calculateScores', true);
}
</script>

<template>
    <div id="record-scores-page">
        <div id="record-scores-container">
            <div id="score-multipliers">
                <span class="water" v-if="selectedRound > 0">
                    <img src="../assets/water.svg" />x3
                </span>
                <span class="plants" v-if="selectedRound > 1">
                    <img src="../assets/plants.svg" />x4
                </span>
                <span class="stone" v-if="selectedRound > 2">
                    <img src="../assets/stone.svg" />x7
                </span>
                <span class="blossoms" v-if="selectedRound > 2">
                    <img src="../assets/blossoms.svg" />1+2+3...
                </span>
            </div>
            <div id="players-container">
                <div v-for="player of players" :key="player.id" class="player-card">
                    <p>{{ player.name }}</p>
                    <div>
                        <span class="water" v-if="selectedRound > 0">
                            <button
                                tabindex="-1"
                                @click="player.scores.water[(selectedRound - 1)]--"
                            >-</button>
                            <input v-model="player.scores.water[(selectedRound - 1)]" inputmode="numeric" />
                            <button
                                tabindex="-1"
                                @click="player.scores.water[(selectedRound - 1)]++"
                            >+</button>
                        </span>
                        <span class="plants" v-if="selectedRound > 1">
                            <button
                                tabindex="-1"
                                @click="player.scores.plants[(selectedRound - 1)]--"
                            >-</button>
                            <input
                                v-model="player.scores.plants[(selectedRound - 1)]"
                                inputmode="numeric"
                            />
                            <button
                                tabindex="-1"
                                @click="player.scores.plants[(selectedRound - 1)]++"
                            >+</button>
                        </span>
                        <span class="stone" v-if="selectedRound > 2">
                            <button
                                tabindex="-1"
                                @click="player.scores.stone[(selectedRound - 1)]--"
                            >-</button>
                            <input v-model="player.scores.stone[(selectedRound - 1)]" inputmode="numeric" />
                            <button
                                tabindex="-1"
                                @click="player.scores.stone[(selectedRound - 1)]++"
                            >+</button>
                        </span>
                        <span class="blossoms" v-if="selectedRound > 2">
                            <button
                                tabindex="-1"
                                @click="player.scores.blossoms[(selectedRound - 1)]--"
                            >-</button>
                            <input
                                v-model="player.scores.blossoms[(selectedRound - 1)]"
                                inputmode="numeric"
                            />
                            <button
                                tabindex="-1"
                                @click="player.scores.blossoms[(selectedRound - 1)]++"
                            >+</button>
                        </span>
                    </div>
                </div>
            </div>
        </div>
        <div id="round-navigation">
            <div>
                <button
                    v-bind:class="selectedRound === 1 ? 'button-primary' : 'button-secondary'"
                    @click="selectRound(1)"
                >1</button>
                <button
                    v-bind:class="selectedRound === 2 ? 'button-primary' : 'button-secondary'"
                    @click="selectRound(2)"
                >2</button>
                <button
                    v-bind:class="selectedRound === 3 ? 'button-primary' : 'button-secondary'"
                    @click="selectRound(3)"
                >3</button>
            </div>
            <button
                id="calculate-button"
                class="button-primary"
                @click="calculateScores()"
            >
                Calculate
            </button>
        </div>
    </div>
</template>

<style scoped lang="scss">
$c_water: #577abd;
$c_plants: #7cc5a6;
$c_stone: #c0c5c2;
$c_blossoms: #ecc3c3;
#record-scores-page {
    margin-top: 24px;
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    #record-scores-container {
        flex: 1;
        display: flex;
        flex-direction: column;
        #score-multipliers {
            display: flex;
            margin-bottom: 12px;
            span {
                display: flex;
                align-items: center;
                margin-right: 12px;
                &.water {
                    color: darken($c_water, 10%);
                }
                &.plants {
                    color: darken($c_plants, 20%);
                }
                &.stone {
                    color: darken($c_stone, 20%);
                }
                &.blossoms {
                    color: darken($c_blossoms, 20%);
                }
                img {
                    height: 48px;
                }
            }
        }
        #players-container {
            flex: 1;
            max-height: calc(100vh - 240px);
            overflow: auto;
            &::-webkit-scrollbar {
                width: 6px;
            }

            &::-webkit-scrollbar-thumb {
                background-color: white;
                border-radius: 6px;
                border: 3px solid rgba(255, 255, 255, 0);
            }
            .player-card {
                padding: 12px;
                margin-right: 12px;
                margin-bottom: 12px;
                background: rgba(white, 0.8);
                border-radius: 12px;
                @media(max-width: 800px) {
                    padding-bottom: 0px;
                    flex: 1;
                    margin-right: 6px;
                }
                p {
                    margin-bottom: 12px;
                }
                div {
                    display: flex;
                    @media(max-width: 800px) {
                        flex-direction: column;
                    }
                    span {
                        flex: 1;
                        max-width: 175px;
                        display: inline-flex;
                        margin-right: 12px;
                        padding: 6px;
                        border-radius: 12px;
                        @media(max-width:800px) {
                            max-width: 100%;
                            margin: 0px 0px 12px 0px;
                        }
                        button {
                            color: white;
                            height: 36px;
                            flex: 1;
                            border-radius: 6px;
                            font-size: 24px;
                            font-weight: 500;
                        }
                        input {
                            width: 60px;
                            height: 36px;
                            border: none;
                            text-align: center;
                            margin: 0px 6px;
                            background: none;
                            border-radius: 6px;
                            @media(max-width:800px) {
                                width: 100%;
                                flex: 1;
                            }
                        }
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
    #round-navigation {
        display: flex;
        justify-content: space-between;
        div {
            button {
                min-width: 48px;
                &.button-secondary,
                &.button-primary {
                    margin-right: 12px;
                    border-radius: 24px;
                    justify-content: center;
                    padding-left: 16px;
                    &::before {
                        border-radius: 27px;
                    }
                    @media(max-width: 360px) {
                        margin-right: 6px;
                    }
                }
            }
        }
        #calculate-button {
            @media (max-width: 800px) {
                letter-spacing: 0px;
                font-size: 16px;
            }
        }
    }
}
</style>
