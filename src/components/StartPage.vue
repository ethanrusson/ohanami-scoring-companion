<script setup>
import { ref } from 'vue'

const emit = defineEmits(['start-score-yourself', 'start-score-group']);

function startScoreYourself(){
    emit('start-score-yourself', false)
}
function startScoreGroup(){
    emit('start-score-group', false)
}
</script>

<template>
    <div id="start-page">
        <button class="button-primary" @click="startScoreYourself()">Score Yourself</button>
        <button class="button-secondary" @click="startScoreGroup()">Score Group</button>
    </div>
</template>

<style scoped lang="scss">
#start-page {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    padding-bottom: 10vh;
}
button {
    display: inline-block;
    margin-top: 12px;
    width: 100%;
    max-width: 300px;
    text-align: center;
}
</style>
