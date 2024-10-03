<script lang="ts">
import { ref, watch, PropType } from 'vue';

import Button from './Button.vue';

export default {
    props: {
        modelValue: {
            type: [String, Number] as PropType<string | number>,
            default: ''
        },
        prefix: String,
        suffix: String,
        icon: String,
        iconPosition: {
            type: String as PropType<'before' | 'after'>,
            default: 'before'
        },
        buttonLabel: String,
        lazy: {
            type: Boolean,
            default: false
        },
        placeholder: String,
        inputClass: {
            type: String,
            default: ''
        }
    },
    components: {
        Button
    },
    emits: ['update:modelValue', 'buttonClick'],
    setup(props, { emit }) {
        const internalValue = ref(props.modelValue);
        watch(() => props.modelValue, (newValue) => {
            internalValue.value = newValue;
        });

        // Handle input event for eager update
        const onInput = (event: Event) => {
            const value = (event.target as HTMLInputElement).value;
            if (!props.lazy) {
                emit('update:modelValue', value);
            }
        };

        // Handle change event for lazy update
        const onChange = (event: Event) => {
            const value = (event.target as HTMLInputElement).value;
            if (props.lazy) {
                emit('update:modelValue', value);
            }
        };

        // Handle button click event
        const onButtonClick = () => {
            emit('buttonClick');
        };

        return {
            internalValue,
            onInput,
            onChange,
            onButtonClick
        };
    }
};
</script>

<template>
    <div class="input" :class="{ 'has-prefix': prefix, 'has-suffix': suffix }">
        <img class="input__icon input__icon--before" v-if="icon && iconPosition === 'before'" :src="icon" alt="">

        <span class="input__prefix" v-if="prefix">{{ prefix }}</span>
        <input 
            class="input__input" 
            :value="modelValue" 
            :style="{ marginRight: buttonLabel ? '1rem' : '' }"
            @input="onInput" 
            @change="onChange" 
            :placeholder="placeholder" />

        <Button v-if="buttonLabel"
            :label="buttonLabel" 
            variant="primary" 
            @buttonClick="onButtonClick"></Button>

        <span class="input__suffix" v-if="suffix">{{ suffix }}</span>

        <img class="input__icon input__icon--before" v-if="icon && iconPosition === 'before'" :src="icon" alt="">
    </div>
</template>

<style scoped>
.input__input {
    position: relative;
    bottom: -1px;
    border: none;
    padding: 0.5rem;
    border-radius: 0.25rem;
    font-size: 1rem;
}
</style>
