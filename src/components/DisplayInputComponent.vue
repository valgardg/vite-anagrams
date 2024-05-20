<template>
    <div class="w-100 d-flex justify-content-between">
        <div v-for="(input, index) in inputs" :key="index" class="display-input-box d-flex justify-content-center align-items-center" :class="[input == '' ? '' : 'filled-input-box']" @click="() => handleRemoveLetter(index)">
            <div class="letter-box">{{ input }}</div>
        </div>
        <div v-for="(_, index) in emptyInputs" :key="index" class="display-input-box d-flex justify-content-center align-items-center">
            <div class="letter-box"></div>
        </div>
    </div>
</template>

<script setup lang="ts">
// imports
import { computed } from 'vue';

// props
const props = defineProps({
    inputs: {
        type: Array,
        required: true,
    },
    handleRemoveLetter: {
        type: Function,
        required: true
    }
});

// constants
const emptyInputs = computed(() => Array.from({length:6-props.inputs.length}));
</script>

<style scoped>
.display-input-box {
    height: 5rem;
    width: 5rem;
    background-color: rgb(105, 74, 120);
    border-radius: 0.4rem;
    cursor: pointer;
}
.filled-input-box {
    background-color: #f5ca3b;
}
.letter-box {
    font-weight: 600;
    font-size: 3rem;
}
</style>