<script setup lang="ts">
import { computed, ComputedRef } from 'vue';

import Button from './components/Button.vue'

function handleButtonClick() {
    alert('Clicked button');
}

const cssVars = computed(() => {
    return {
        '--primary-color'     : '#007bff',
        '--secondary-color'   : '#2e6e49',
        '--border-hover-color': '#646cff'
    }
});
</script>

<template>
    <div class="buttons">
        <Button 
            :style="cssVars"
            label="Test Button" 
            icon="./assets/star.svg" 
            iconPosition="before" 
            variant="primary" 
            @click="() => handleButtonClick()"></Button>

        <Button 
            :style="cssVars"
            label="No icon button"
            variant="secondary"></Button>
    
        <Button 
            :style="cssVars"
            label="Hyperlink button" 
            icon="./assets/arrow.svg" 
            iconPosition="after" 
            href="https://www.google.com"></Button>
    </div>
</template>

<style scoped>
.buttons {
    display: flex;
    gap: 1rem;
}
</style>
