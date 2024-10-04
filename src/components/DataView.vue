<script lang="ts">
import { ref, computed, PropType } from 'vue';

import Button from './Button.vue';
import Input from './Input.vue';

export default {
    props: {
        cssVars: Object,
        style: Object,
        mode: {
            type: String as PropType<'table' | 'grid'>,
            default: 'table'
        },
        sortBy: {
            type: String as PropType<'name asc' | 'name desc'>,
            default: 'name asc'
        },
        search: Boolean,
        selectable: Boolean,
        actions: Array<{
            label: string,
            icon: string,
            action: () => void
        }>,
        items: {
            type: Array as PropType<Array<{
                image: string,
                name: string,
                description: string,
                style: Record<string, string>
            }>>,
            default: []
        }
    },
    components: {
        Button,
        Input
    },
    emits: ['buttonClick'],
    setup(props, { emit }) {
        const onButtonClick = () => {
            emit('buttonClick');
        };

        // search
        const query = ref('');
        const onInput = (value: string) => {
            query.value = value;
        };

        // list mode
        const mode = ref(props.mode);
        const updateMode = (event: Event) => {
            mode.value = (event.target as HTMLInputElement).value as 'table' | 'grid';
        };

        // sort
        const sortBy = ref(props.sortBy);
        const updateSortBy = (event: Event) => {
            sortBy.value = (event.target as HTMLInputElement).value as 'name asc' | 'name desc';
        };

        // select
        const selected = ref<string[]>([]);
        const select = (id: string) => {
            if (selected.value.includes(id)) {
                selected.value = selected.value.filter((item) => item !== id);
            } else {
                selected.value = [...selected.value, id];
            }
        };

        // select all
        const selectAll = () => {
            console.log('selectAll');
            if (selected.value.length === props.items.length) {
                selected.value = [];
            } else {
                selected.value = props.items.map((item) => 'selected-' + item.name);
            }
        };

        // Computed property for filtered items
        const filteredItems = computed(() => {
            let filtered = props.items;

            if (query.value) {
                filtered = filtered.filter((item) => {
                    return item.name.toLowerCase().includes(query.value.toLowerCase());
                });
            }

            if (sortBy.value === 'name asc') {
                filtered = filtered.sort((a, b) => a.name.localeCompare(b.name));
            } else {
                filtered = filtered.sort((a, b) => b.name.localeCompare(a.name));
            }

            return filtered;
        });

        return {
            mode,
            sortBy,
            filteredItems,
            selected,
            onInput,
            onButtonClick,
            updateMode,
            updateSortBy,
            select,
            selectAll
        };
    }
};
</script>

<template>
    <form class="filters" @submit="event => event.preventDefault()">
        <Input 
            v-on:update:modelValue="value => onInput(value)"
            placeholder="Search for name"/>
        <div class="filters__checkboxes">
            <label class="filters__label">
                <span>Table</span>
                <input type="radio" name="mode" id="mode-table" value="table" :checked="mode === 'table'" @click="updateMode">
            </label>
            <label class="filters__label">
                <span>Grid</span>
                <input type="radio" name="mode" id="mode-grid" value="grid" :checked="mode === 'grid'" @click="updateMode">
            </label>
        </div>

        <div class="filters__selects">
            <label class="filters__label">
                <span>Sort:</span>
                <select class="filters__select" name="sortBy" id="sortBy" :value="sortBy" @change="updateSortBy">
                    <option value="name asc">Name asc</option>
                    <option value="name desc">Name desc</option>
                </select>
            </label>
        </div>

        <!-- Select all button -->
        <Button
            v-if="selectable"
            :style="cssVars"
            :label="'Select all'"
            variant="primary"
            @buttonClick="selectAll">
        </Button>
    </form>
    <ul :class="['data-view', 'data-view--' + mode]">
        <li class="data-view__item" v-for="item of filteredItems" :style="item.style">
            <div class="data-view__content">
                <img v-if="item.image" class="data-view__image" :src="item.image" alt="">
                <div class="data-view__details">
                    <h3 class="data-view__name">{{ item.name }}</h3>
                    <p class="data-view__description">{{ item.description }}</p>
                </div>
            </div>
            <div class="data-view__actions">
                <button v-for="action of actions" @click="action.action" class="data-view__action">{{ action.label }}</button>
                <input
                    class="data-view__select-button"
                    type="checkbox"
                    name="selected"
                    :id="'selected-' + item.name"
                    :value="'selected-' + item.name"
                    v-model="selected">
            </div>
        </li>
    </ul>
</template>

<style scoped>
.filters {
    display: flex;
    margin-bottom: 1rem;
}
@media screen and (max-width: 600px) {
    .filters {
        flex-direction: column;
    }
}

.filters__checkboxes,
.filters__selects {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 0 1rem;
}
@media screen and (max-width: 600px) {
    .filters__checkboxes,
    .filters__selects {
        margin-bottom: 1rem;
    }
}

.filters__label,
.filters__label input {
    cursor: pointer;
}
.filters__select {
    position: relative;
    bottom: -1px;
    border: none;
    padding: 0.7rem;
    border-radius: 0.25rem;
    font-size: 1rem;
    margin-left: 1rem;
}

.data-view {
    display: flex;
    flex-wrap: wrap;
    list-style: none;
    padding: 0;
    margin: 0;
    gap: 1rem;
}
.data-view--grid .data-view__item {
    flex-direction: column;
    width: calc(100% / 3 - 2.67rem);
    padding-bottom: 1rem;
}
.data-view--grid .data-view__actions {
    display: flex;
    align-items: center;
}
.data-view--grid .data-view__action:not(:last-child) {
    margin-bottom: 0;
    margin-right: 0.5rem;
}

.data-view__content {
    display: flex;
    align-items: center;
}

.data-view__item {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: space-between;
    background-color: #f8f9fa;
    color: black;
    width: 100%;
    padding: 0 1rem;
    border-radius: 1rem;
}
@media screen and (max-width: 800px) {
    .data-view--grid .data-view__item {
        width: calc(100% / 2 - 2.67rem);
    }
}
@media screen and (max-width: 600px) {
    .data-view--grid .data-view__item {
        width: 100%;
    }
}

.data-view__select-button {
    cursor: pointer;
}

.data-view__image {
    width: 50px;
    height: 50px;
    margin-right: 1rem;
}

.data-view__name {
    margin: 1rem 0 0.25rem;
}

.data-view__description {
    margin: 0.25rem 0 1rem;
}

.data-view__actions {
    text-align: right;
}

.data-view__action {
    display: block;
    width: 100%;
    cursor: pointer;
}

.data-view__action:not(:last-child) {
    margin-bottom: 0.5rem;
}
</style>
