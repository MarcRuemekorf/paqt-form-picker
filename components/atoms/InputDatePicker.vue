<template>
  <component :is="htmlTag">
    <label
      v-if="label"
      :for="name"
      class="block mb-1 text-sm"
    >
      {{ label }} <span v-if="required" class="text-red-600">*</span>
    </label>
    <vue-pikaday
      v-model="inputValue"
      :id="name"
      placeholder="Pick a date"
      :options="options"
      @input="$emit('input', inputValue)"
      @blur="$emit('blur')"
      :class="['w-full px-3 py-2 sm:text-sm border-gray-200 shadow-sm rounded border border-gray-300', errors.length ? 'border-red-500 text-red-600 focus:ring-red-500 focus:border-red-500 placeholder-red-400' : 'focus:ring-blue-500 focus:border-blue-500 placeholder-gray-400']"
    />
    <!-- Helper text -->
    <div v-if="helper" class="mt-1 text-sm text-gray-400">
      {{ helper }}
    </div>
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
import {ref, watch} from '@nuxtjs/composition-api'
export default {
  name: 'InputDatePicker',
  props: {
    htmlTag: {
      type: String,
      default: 'div'
    },
    value: {
      type: Date,
      default: ''
    },
    name: {
      type: String,
      default: ''
    },
    label: {
      type: String,
      default: ''
    },
    placeholder: {
      type: String,
      default: ''
    },
    required: {
      type: Boolean,
      default: false
    },
    helper: {
      type: String,
      default: ''
    },
    errors: {
      type: Array,
      default: () => []
    },
    options: {
      type: Object,
      default: () => {}
    }
  },
  setup(props) {
    const inputValue = ref(props.value)

    watch(
      () => props.value,
      (value) => {
        inputValue.value = value
      }
    )

    return {
      inputValue
    }
  }
}
</script>
