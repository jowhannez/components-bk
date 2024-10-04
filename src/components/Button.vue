<script lang="ts">
import { PropType } from 'vue';

export default {
    props: {
        cssVars: Object,
        style: Object,
        label: String,
        href: String,
        icon: String,
        iconPosition: {
            type: String as PropType<'before' | 'after'>,
            default: 'before'
        },
        variant: {
            type: String as PropType<'primary' | 'secondary'>,
            default: 'primary'
        }
    },
    emits: ['buttonClick'],
    setup(_, { emit }) {
        // Handle button click event
        const onButtonClick = () => {
            emit('buttonClick');
        };

        return {
            onButtonClick
        };
    }
};
</script>

<template>
    <component :is="href ? 'a' : 'button'" :href="href" :target="href ? '_blank' : null"
        :style="style"
        :rel="href ? 'noopener noreferrer' : null" @click="onButtonClick"
        :class="['button', `button--${variant}`, { 'button--has-icon': icon }]"
        :aria-label="href ? 'Opens in new tab' : ''">

        <img v-if="icon && iconPosition === 'before'" class="button__icon button__icon--before" :src="icon" alt="">
        <span class="button__label" v-if="label">{{ label }}</span>
        <img v-if="icon && iconPosition === 'after'" class="button__icon button__icon--after" :src="icon" alt="">
    </component>
</template>

<style scoped>
.button {
    display: inline-flex;
    align-items: center;
    cursor: pointer;
    border: none;
    padding: 0.5rem;
    border-radius: 0.25rem;
    font-size: 1rem;
}

.button:hover,
.button:focus,
.button:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
}

.button--primary {
    background-color: var(--primary-color);
    color: white;
}

.button--secondary {
    background-color: var(--secondary-color);
    color: white;
}

.button__icon {
    width: 1.5rem;
    height: 1.5rem;
}

.button__icon--before {
    margin-right: 1rem;
}

.button__icon--after {
    margin-left: 1rem;
}
</style>
