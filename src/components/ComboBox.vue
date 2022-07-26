<template>
    <div class="flex flex-col items-center">
        <div class="flex items-center gap-5">
            <Combobox v-model="selected" class="w-56">
                <div class="relative mt-1">
                    <div class="relative w-full cursor-default overflow-hidden rounded-lg bg-white text-left shadow-md focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75 focus-visible:ring-offset-2 focus-visible:ring-offset-teal-300 sm:text-sm">
                        <ComboboxInput id="comboBoxInput" class="w-full border-none p-2 text-base leading-5 text-gray-900 focus:ring-0" @change="query = $event.target.value" />
                    </div>
                    <ComboboxOptions v-if="filteredPeople.length !== 0" class="absolute mt-1 max-h-60 w-full overflow-auto rounded-md bg-white py-1 text-base shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none sm:text-sm">
                        <div v-if="filteredPeople.length === 0 && query !== ''" class="relative cursor-default select-none py-2 px-4 text-gray-700">
                            Nothing found.
                        </div>
                        <ComboboxOption v-for="person in filteredPeople" :key="person" :value="person">
                            <li class="relative cursor-default select-none py-2 pl-10 pr-4 bg-white text-gray-900 hover:text-white hover:bg-gradient-to-r hover:from-gray-800 hover:to-gray-900">
                                <span class="block truncate" :class="{ 'font-medium': selected, 'font-normal': !selected }">
                                    {{ person }}
                                </span>
                            </li>
                        </ComboboxOption>
                    </ComboboxOptions>
                </div>
            </Combobox>
            <i class="fa fa-circle-check text-gray-900 hover:text-green-600 cursor-pointer" @click="callback(comboBoxInput.value); comboBoxInput.value = ''"></i>
        </div>
    </div>
</template>

<script setup>
import { computed } from 'vue'
import { Combobox, ComboboxInput, ComboboxOptions, ComboboxOption } from '@headlessui/vue'
</script>

<script>

export default {
    name: 'ComboBox',
    props: {
        people: {
            type: Array,
            required: true,
        },
        callback: {
            type: Function,
            required: true
        }
    },
    data() {
        return {
            selected: '',
            query: '',
            filteredPeople: [],
            comboBoxInput: HTMLElement
        }
    },
    methods: {
    },
    mounted() {
        this.comboBoxInput = document.getElementById('comboBoxInput')
        this.filteredPeople = computed(() => {
                return this.query === '' ? this.people : this.people.filter(person => { return person.toLowerCase().includes(this.query.toLowerCase()) })
        })
    }
}
</script>

<style>
.fa-circle-check:before {
    content: '\f058';
    font-size: 2rem;
}
</style>