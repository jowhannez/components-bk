<script lang="ts">
import { ref, watch, PropType } from 'vue';

import Button from './Button.vue';

export default {
    props: {
        cssVars: Object,
        style: Object,
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
        lazy: {
            type: Boolean,
            default: false
        },
        placeholder: String,
        inputClass: {
            type: String,
            default: ''
        },
        button: Object
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

        <span v-if="prefix" class="input__prefix">{{ prefix }}</span>
        <input 
            class="input__input" 
            :value="modelValue" 
            :style="{ ...style, ...{ marginRight: button && prefix ? '1rem' : '' } }"
            @input="onInput" 
            @change="onChange" 
            :placeholder="placeholder" />
        <span v-if="suffix"
            class="input__suffix" 
            :style="{ marginRight: button && suffix ? '1rem' : '' }">{{ suffix }}</span>

        <Button v-if="button"
            :style="cssVars"
            :label="button.label" 
            :href="button.href"
            :icon="button.icon"
            :iconPosition="button.iconPosition"
            :variant="button.variant"
            @buttonClick="onButtonClick"></Button>

        <img class="input__icon input__icon--before" v-if="icon && iconPosition === 'before'" :src="icon" alt="">
    </div>
</template>

<style scoped>
.input__input {
    position: relative;
    bottom: -1px;
    border: none;
    padding: 0.7rem;
    border-radius: 0.25rem;
    font-size: 1rem;
}

@media screen and (max-width: 600px) {
    .input__input {
        width: 80%;
        margin-bottom: 1rem;
    }
}
.input__prefix,
.input__suffix {
    margin: 0 0.5rem;
}
</style>
