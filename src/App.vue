<script setup lang="ts">
import { computed, watch, ref } from 'vue';

import Button from './components/Button.vue';
import Input from './components/Input.vue';

const cssVars = computed(() => {
    return {
        '--primary-color'     : '#007bff',
        '--secondary-color'   : '#2e6e49',
        '--border-hover-color': '#646cff'
    }
});

const inputValue     = ref('');
const lazyInputValue = ref('');

watch(() => inputValue, (newValue) => {
    console.log('Input value:', newValue);
});
watch(() => lazyInputValue, (newValue) => {
    console.log('Lazy input value:', newValue);
});

function handleButtonClick() {
    alert('Clicked button');
}

</script>

<template>
    <h1>Noen Vue-components</h1>
    <h2>Buttons</h2>
    <p>Her definerte jeg bare noen CSS-variabler for farger i rota og sender inn som en computed prop. 
        Jeg har løst oppgaven ved å bruke vanlige props og ved å emitte en custom event "buttonClick" når
        knappen klikkes. Det er mulig å sette inn icon via path/url, bestemme posisjon av ikonet (before/after). 
        I tillegg er det to varianter med forskjellige stiler (primary/secondary)</p>
    <div class="buttons">
        <Button 
            :style="cssVars"
            label="Test Button" 
            icon="./assets/star.svg" 
            iconPosition="before" 
            variant="primary" 
            @buttonClick="() => handleButtonClick()"></Button>

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

    <h2>Inputs</h2>
    <p>Css løst på samme måte som buttons. Jeg har gjenbrukt Button-componenten her som "integrated button" fra oppgaveteksten.</p>
    <div class="inputs">
        <Input 
            :style="cssVars"
            placeholder="Snappy search..."
            v-model="inputValue" 
            buttonLabel="Search" 
            @buttonClick="handleButtonClick">
            <component :is="<Button
            >"></component>
        </Input>
        <div class="inputs__result">{{ inputValue }}</div>

        <Input 
            :style="cssVars"
            placeholder="Lazy search..."
            v-model="lazyInputValue" 
            :lazy="true"></Input>
        <div class="inputs__result">{{ lazyInputValue }}</div>
    </div>
</template>

<style scoped>
.buttons {
    display: flex;
    justify-content: center;
    gap: 1rem;
}
.inputs {
    margin-top: 1.5rem;
}
.inputs__result {
    min-height: 1.5rem;
}
</style>
