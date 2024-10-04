<script setup lang="ts">
import { computed, ref } from 'vue';

import Button from './components/Button.vue';
import Input from './components/Input.vue';
import DataView from './components/DataView.vue';

const cssVars = computed(() => {
    return {
        '--primary-color'     : '#007bff',
        '--secondary-color'   : '#2e6e49',
        '--border-hover-color': '#646cff'
    }
});

const inputValue     = ref('');
const lazyInputValue = ref('');

interface DataItem {
    image: string;
    name: string;
    description: string;
    style: Record<string, string>;
}
const dummyData: DataItem[] = [];
for (let i = 0; i < 10; i++) {
    dummyData.push({
        image: i % 3 === 0 ? '/assets/star.svg' : '',
        name: Math.random().toString(36).substring(7),
        description: 'A star is a luminous sphere of plasma held together by its own gravity.',
        style: {}
    });
}

function handleButtonClick(text: string = 'Clicked button') {
    alert(text);
}

</script>

<template>
    <h1>Noen Vue-components</h1>
    <p>Her har jeg ikke lagd funksjonalitet for hva som faktisk skal skje når man trykker på knapper og fyller
        inn tekst i inputs. Dette er kun for å vise hvordan jeg har lagd komponentene og hvordan de kan brukes.
        Tanken er at man skriver funksjonaliteten i App.vue eller i en annen komponent som bruker disse komponentene.
        Jeg har valgt å skrive komponentene med composition api logikk i setup-funksjoner men har strukturert de mer
        som i Options-API'et da jeg synes strukturen ser bedre ut og er litt mer oversiktlig.
    </p>

    <h2>Buttons</h2>
    <p>Her definerte jeg bare noen CSS-variabler for farger i rota og sender inn som en computed prop. 
        Jeg har løst oppgaven ved å bruke vanlige props og ved å emitte en custom event "buttonClick" når
        knappen klikkes. Det er mulig å sette inn icon via path/url, bestemme posisjon av ikonet (before/after). 
        I tillegg er det to varianter med forskjellige stiler (primary/secondary)</p>

    <section class="buttons">
        <Button 
            :style="cssVars"
            variant="primary" 
            label="Test Button" 
            icon="./assets/star.svg" 
            iconPosition="before" 
            @buttonClick="() => handleButtonClick('Clicked test button!')"></Button>

        <Button 
            :style="{...cssVars, ...{'background-color': '#f35050'}}"
            label="No icon button"></Button>
        
        <Button 
            :style="cssVars"
            variant="secondary" 
            label="Hyperlink button" 
            icon="./assets/arrow.svg" 
            iconPosition="after" 
            href="https://www.google.com"></Button>
    </section>

    <h2>Inputs</h2>
    <p>Her kan man sende button props inn til input-komponenten som et objekt, og så blir button-komponenten 
        jeg lagde tidligere brukt til å rendere knappen. Man kan sende inn suffix og prefix, placeholder, 
        definere v-model variabel og hva som skal skje når man trykker på knappen. Skriv inn tekst for å se
        eager/lazy oppdatering</p>

    <section class="inputs">
        <Input 
            :button="{
                label: 'Search',
                icon: './assets/search.svg',
                iconPosition: 'after',
                variant: 'primary',
            }"
            :cssVars="cssVars"
            :prefix="'$'"
            placeholder="Snappy search..."
            v-model="inputValue"
            @buttonClick="() => handleButtonClick()">
        </Input>
        <div class="inputs__result">{{ inputValue }}</div>

        <Input 
            :cssVars="cssVars"
            placeholder="Lazy search..."
            v-model="lazyInputValue" 
            :suffix="'kr,-'"
            :lazy="true"></Input>
        <div class="inputs__result">{{ lazyInputValue }}</div>
    </section>

    <h2>Data view</h2>
    <p>Her har valgt å ikke implementere action-funksjonaliteten så de er bare til demonstrasjon. Det er 
        mulig å sortere dataene, søke etter data, velge visningsmodus (tabell/grid) og velge alle boksene.
        Dataene er laget tilfeldig for demonstrasjon. Velger å ikke bruke en haug av tid på nettsideoptimalisering
        og responsitivitet da det ikke er en del av oppgaven.
    </p>
    <section class="data-view">
        <DataView 
            :cssVars="cssVars"
            :items="dummyData"
            mode="table",
            sortBy="name asc"
            :search="true"
            :selectable="true"
            :actions="[
                { 
                    label: 'Edit', 
                    action: () => handleButtonClick('Clicked edit')
                },
                { 
                    label: 'Delete', 
                    action: () => handleButtonClick('Clicked edit')
                }
            ]"></DataView>        
    </section>
</template>

<style scoped>
.buttons {
    display: flex;
    gap: 1rem;
}
.inputs {
    margin-top: 1.5rem;
}
.inputs__result {
    min-height: 1.5rem;
}
</style>
