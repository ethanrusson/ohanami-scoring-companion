<script setup>
import { ref } from 'vue';
import StartPage from './components/StartPage.vue';
import AddPlayersPage from './components/AddPlayersPage.vue';
import RecordScores from './components/RecordScores.vue';
import CalculateScores from './components/CalculateScores.vue';

let onStartPage = ref(true);
let onAddPlayers = ref(false);
let onStartScoring = ref(false);
let onCalculateScores = ref(false);
let scoreYourself = ref(false);
let scoreGroup = ref(true);
let players = ref([]);
function returnToStartPage() {
  onStartPage.value = true;
  onAddPlayers.value = false;
  scoreYourself.value = false;
  onStartScoring.value = false;
  onCalculateScores.value = false
}
function startScoreYourself(e) {
  onStartPage.value = e;
  scoreYourself.value = true;
  startScoring([]);
}
function startScoreGroup(e) {
  onStartPage.value = e;
  onAddPlayers.value = true;
  scoreGroup.value = true;
}
function startScoring(e) {
  if (e.length) {
    players.value = e;
  } else {
    players.value = [
      {
        id: 0,
        name: 'Player',
        scores: {
          'water': [0, 0, 0],
          'plants': [0, 0, 0],
          'stone': [0, 0, 0],
          'blossoms': [0, 0, 0]
        }
      }
    ]
  }
  onStartPage.value = false;
  onAddPlayers.value = false;
  onStartScoring.value = true;
  onCalculateScores.value = false;
}
function calculateScores(e) {
  players.value.forEach(player => {
    player.scores.water[3] = (player.scores.water[0] * 3) + (player.scores.water[1] * 3) + (player.scores.water[2] * 3);
    player.scores.plants[3] = (player.scores.plants[1] * 4) + (player.scores.plants[2] * 4);
    player.scores.stone[3] = (player.scores.stone[2] * 7);
    player.scores.blossoms[3] = 0;
    for(let i = 1; i <= player.scores.blossoms[2]; i++) {
      player.scores.blossoms[3] = player.scores.blossoms[3] + i;
    }
    player.totalScore = player.scores.water[3] + player.scores.plants[3] + player.scores.stone[3] + player.scores.blossoms[3];
  });
  players.value.sort((a, b) => (a.totalScore < b.totalScore) ? 1 : -1);
  onStartPage.value = false;
  onAddPlayers.value = false;
  onStartScoring.value = false;
  onCalculateScores.value = true;
}
function returnToScoring(e) {
  onStartScoring.value = true;
  onCalculateScores.value = false;
}
</script>

<template>
  <header v-bind:class="{ startpage: onStartPage }">
    <div id="vertical-header" v-if="onStartPage">
      <img id="ohanami-logo-vertical" src="./assets/ohanami-vertical.svg" />
      <p>Unofficial</p>
      <h1>Scoring Companion</h1>
    </div>
    <div id="horizontal-header" v-if="!onStartPage">
      <img id="ohanami-logo-horizontal" src="./assets/ohanami-horizontal.svg" />
      <button class="button-secondary" @click="returnToStartPage()">
        <img src="./assets/restart.svg" />
      </button>
    </div>
  </header>
  <StartPage
    v-if="onStartPage"
    @start-score-yourself="startScoreYourself"
    @start-score-group="startScoreGroup"
  />
  <AddPlayersPage 
    v-if="onAddPlayers" 
    @players="startScoring"
  />
  <RecordScores 
    v-if="onStartScoring" 
    @calculateScores="calculateScores" 
    v-model:players="players" 
  />
  <CalculateScores 
    v-if="onCalculateScores" 
    @returnToScoring="returnToScoring" 
    :players="players"
  />
</template>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Work+Sans:ital,wght@0,300;0,500;0,700;1,500&display=swap");
$c_primary: #b15b55;
#app {
  display: flex;
  flex-direction: column;
  height: -webkit-fill-available;
  max-width: 800px;
  margin: 0 auto;
  padding: 12px;
  overflow: auto;
  @media(max-width: 800px) {
    padding: 12px 12px 0px 12px;
  }
  &::-webkit-scrollbar {
      width: 6px;
  }
  &::-webkit-scrollbar-thumb {
      background-color: rgba(255,255,255,0.6);
      border-radius: 6px;
      border: 3px solid rgba(255, 255, 255, 0);
  }
  & *::selection {
    background-color: rgba($c_primary, 0.24);
  }
  h1 {
    font-size: 24px;
    font-weight: 500;
  }
  input {
    font-size: 18px;
    &:focus {
      outline: 2px solid $c_primary;
    }
    &::-webkit-outer-spin-button,
    &::-webkit-inner-spin-button {
      -webkit-appearance: none;
      margin: 0;
    }
    &[type="number"] {
      -moz-appearance: textfield;
    }
  }
  button {
    outline: none;
    border: none;
    border-radius: 12px;
    height: 48px;
    padding: 0px 12px;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    font-size: 18px;
    text-transform: uppercase;
    letter-spacing: 4px;
    cursor: pointer;
    transition: 0.2s ease;
    background: white;
    position: relative;
    touch-action: manipulation;
    &::before {
      content: "";
      position: absolute;
      opacity: 0;
      left: -4px;
      top: -4px;
      width: calc(100% + 4px);
      height: calc(100% + 4px);
      border: 2px solid $c_primary;
      border-radius: 15px;
      pointer-events: none;
    }
    &.button-primary {
      background: $c_primary;
      color: white;
      &:hover {
        background: rgba($c_primary, 0.6);
        text-shadow: 0px 1px 2px rgba(0, 0, 0, 0.24);
      }
      &:focus::before {
        opacity: 1;
      }
      &:active {
        background: rgba($c_primary, 0.24);
      }
    }
    &.button-secondary {
      background: white;
      color: $c_primary;
      &:hover {
        background: rgba(white, 0.6);
      }
      &:focus::before {
        opacity: 1;
      }
      &:active {
        background: rgba(white, 0.24);
      }
    }
  }
  header {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    &.startpage {
      flex: 1;
    }
    p {
      margin-top: 24px;
      font-size: 18px;
    }
    h1 {
      margin-bottom: 12px;
      padding-bottom: 24px;
      border-bottom: 2px solid rgba(white, 0.25);
      width: 100%;
    }
    #vertical-header {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
    }
    #horizontal-header {
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }
  }
}
#ohanami-logo-vertical {
  max-height: 40vh;
  @media(max-width: 800px) {
    max-height: 30vh;
  }
}
#ohanami-logo-horizontal {
  height: 48px;
  max-width: calc(100% - 60px);
}
</style>
