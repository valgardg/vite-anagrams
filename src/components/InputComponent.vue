<template>
    <div class="w-100 d-flex justify-content-between">
        <div v-for="(input, index) in letters" :key="index" class="d-flex justify-content-center align-items-center" :class="[!(props.isPlaying) ? 'input-box-disabled' : (props.usedInputs.includes(index) ? 'input-box-selected' : 'input-box')]" @click="() => isPlaying ? props.handleInput(input, index) : {}">
            <div v-if="!props.usedInputs.includes(index)" class="letter-box">{{ input }}</div>
        </div>
    </div>
</template>

<script setup lang="ts">
// props
const props = defineProps({
    letters: {
        type: Array,
        require: true
    },
    handleInput: {
        type: Function,
        required: true
    },
    usedInputs: {
        type: Array,
        required: true
    },
    isPlaying: {
        type: Boolean,
        required: true
    }
});

</script>

<style scoped>
.input-box {
    height: 5rem;
    width: 5rem;
    background-color: #f5ca3b;
    border-radius: 0.4rem;
    cursor: pointer;
}
.input-box-selected {
    height: 5rem;
    width: 5rem;
    background-color: transparent;
    border-radius: 0.4rem;
    border-style: solid;

}
.input-box-disabled {
    height: 5rem;
    width: 5rem;
    background-color: #eccb5f;
    border-radius: 0.4rem;
}
.letter-box {
    font-weight: 600;
    font-size: 3rem;
}
</style>