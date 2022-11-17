<script setup>
import { computed } from "@vue/runtime-core";
import Player from './Player.vue';

const props = defineProps({
    courtData: {
        type: Object,
        required: true,
    },
});

const players = computed(() => props.courtData.players);

const player1 = players.value.length ? players.value[0]?.name : "";
const player2 = players.value.length > 1 ? players.value[1]?.name : "";
const player3 = players.value.length > 2 ? players.value[2]?.name : "";
const player4 = players.value.length > 3 ? players.value[3]?.name : "";
</script>

<template>
    <div class="court">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <Player :player-name="player1" />
        <div></div>
        <div></div>
        <Player :player-name="player2" />
        <div></div>
        <div></div>
        <Player :player-name="player3" />
        <Player :player-name="player4" />
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </div>
</template>

<style scoped>
.court {
    display: grid;
    grid-template-rows: 40px 60px 60px 40px;
    grid-template-columns: 50px 200px 80px 80px 200px 50px;
    border: solid 1px black;
    justify-content: center;
    align-content: center;
    transform: scale(0.75);
}
.court > div {
    border: solid black;
    border-width: 1px 0 0 1px;
    transition: all 0.5s;
}
.court > div:nth-child(6),
.court > div:nth-child(12),
.court > div:nth-child(16),
.court > div:nth-child(22) {
    border-right-width: 1px;
}
.court > div:nth-last-child(-n + 6) {
    border-bottom-width: 1px;
}
.court > div:nth-child(9),
.court > div:nth-child(10) {
    grid-row: 2/4;
}
.court > div:nth-child(9) {
    grid-column: 3;
}
.court > div:nth-child(10) {
    grid-column: 4;
}
</style>