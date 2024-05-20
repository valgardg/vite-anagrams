<template>
    <div class="d-flex justify-content-center">
        <div class="display-score-container d-flex justify-content-center align-items-center">
            <div class="mx-2">
                <img src="https://i.pinimg.com/736x/8b/b0/ce/8bb0ceb5e1e698f0051f5c59de976ebc.jpg" class="profile-picture"/>
            </div>
            <div class="mx-2">
                <div class="words-text">WORDS:{{ props.currentWords }}</div>
                <div class="score-text">SCORE:{{ props.currentScore }}</div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">

// props
const props = defineProps({
    currentScore: {
        type: String,
        required: true
    },
    currentWords: {
        type: String,
        required: true
    }
});
</script>

<style scoped>
.display-score-container {
    width: 75%;
    background-image: url('https://t4.ftcdn.net/jpg/05/34/87/15/360_F_534871551_MOmx3mu3oP1TkmUW8ZDffLpHrv86LLrE.jpg');
}
.profile-picture {
    height: 4.5rem;
    width: 4.5rem;
    border-radius: 50%;
}
.words-text {
    font-weight: 800;
    font-size:1.5rem;
}
.score-text {
    font-weight: 800;
    font-size: 2.5rem;
}
</style>