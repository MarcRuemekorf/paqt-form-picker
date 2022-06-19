<template>
  <component :is="htmlTag">
    <fieldset>
      <legend v-if="legend" class="text-base font-medium text-gray-900 mb-3">{{ legend }}</legend>
      <div class="space-y-4">
        <template v-for="(option, index) in options">
          <!--
            Checked: "border-blue-500 ring-2 ring-blue-500", Not Checked: "border-gray-300"
          -->
          <label :class="['relative block bg-white border rounded shadow-sm px-6 py-4 cursor-pointer sm:flex sm:justify-between focus:outline-none', option.value === selected ? 'border-blue-500 ring-2 ring-blue-500' : 'border-gray-300']" :key="index" @click="select(option.value)">
            <input type="radio" :name="option.label.toLowerCase()" :value="option.value" class="sr-only" :aria-labelledby="option.label.toLowerCase() + '-label'" aria-describedby="server-size-1-description-0 server-size-1-description-1">
            <span class="flex items-center">
              <span class="text-sm flex flex-col">
                <span :id="option.label.toLowerCase() + '-label'" class="font-medium text-gray-900">{{ option.label }}</span>
                <span v-if="option.description" :id="option.label.toLowerCase() + '-description-0'" class="text-gray-500">
                  <span class="block sm:inline">{{ option.description }}</span>
                </span>
              </span>
            </span>
            <span :id="option.label.toLowerCase() + '-description-1'" class="mt-2 flex text-sm sm:mt-0 sm:flex-col sm:ml-4 sm:text-right">
              <span class="font-medium text-gray-900">€{{ option.price }}</span>
              <span class="ml-1 text-gray-500 sm:ml-0">p/m</span>
            </span>
            <!--
            Checked: "border-blue-500", Not Checked: "border-transparent"
          -->
            <span :class="['absolute -inset-px rounded border pointer-events-none', option.value === selected ? 'border-blue-500' : 'border-transparent']" aria-hidden="true"></span>
          </label>
        </template>
      </div>
    </fieldset>

    <!-- Helper text -->
    <div v-if="helper" class="mt-1 text-sm text-gray-400">
      {{ helper }}
    </div>
    <!-- Error text -->
    <ul v-if="errors.length" class="mt-1 text-sm text-red-500">
      <li v-for="(error, index) in errors" :key="index" class="flex items-start">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1 mt-0.5 flex-shrink-0" viewBox="0 0 20 20" fill="currentColor">
          <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd" />
        </svg>
        {{ error }}
      </li>
    </ul>
  </component>
</template>
<script>
import { ref } from '@nuxtjs/composition-api';
export default {
  name: 'InputRadioGroup',
  props: {
    htmlTag: {
      type: String,
      default: 'div'
    },
    value: {
      type: Number,
      default: null
    },
    legend: {
      type: String,
      default: ''
    },
    options: {
      type: Array,
      default: () => [],
      note: 'Objects in array must include: label (String), description (String), price (String), value (number)'
    },
    helper: {
      type: String,
      default: ''
    },
    errors: {
      type: Array,
      default: () => []
    }
  },
  setup(props, {emit}) {
    const selected = ref(props.value)
    const select = (value) => {
      selected.value = value
      emit('input', value)
    }

    return {
      selected,
      select
    }
  }
}
</script>
