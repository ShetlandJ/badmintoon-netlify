<script setup>
defineProps({
    playerName: {
        type: String,
        required: true,
    },
});
</script>

<template>
    <div
        class="
            flex
            justify-center
            items-center
        "
        :class="{
            'text-md': playerName.length > 20,
            'text-xl': playerName.length < 20 && playerName.length > 15,
            'text-2xl': playerName.length < 10,
        }"
    >
        {{ playerName }}
    </div>
</template>