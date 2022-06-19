<template>
  <div class="bg-white py-8 px-4 shadow sm:rounded-lg sm:px-10">
    <form class="space-y-6" action="#" method="POST">
      <!-- Servers -->
      <InputRadioGroup
        v-model="servers.selected"
        legend="Servers"
        :options="servers.options"
        :errors="formErrors ? formErrors.serverId : ''"
      />

      <!-- Duration -->
      <div class="grid grid-cols-2 gap-3">
        <div class="sm:col-span-1">
          <!-- Start date -->
          <InputDatePicker
            v-model="startDate.selected"
            name="start-date"
            label="Start datum"
            :options="startDate.options"
            helper="Selecteer startdatum"
            :errors="formErrors ? formErrors.startDate : ''"
            @blur="onStartDateBlur()"
          />
        </div>

        <div class="sm:col-span-1">
          <!-- End date -->
          <InputDatePicker
            v-model="endDate.selected"
            name="end-date"
            label="Eind datum"
            :options="endDate.options"
            helper="Selecteer einddatum"
            :errors="formErrors ? formErrors.endDate : ''"
          />
        </div>
      </div>

      <!-- Form actions -->
      <div>
        <button type="button" class="w-full flex justify-center py-2 px-4 border border-transparent rounded shadow-sm text-sm font-medium text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500" @click="checkAvailability">Controleer beschikbaarheid</button>
      </div>
    </form>

    <!-- Form message -->
    <div v-if="message.text" class="mt-6">
      <div :class="['p-3 text-sm rounded', message.type === 'error' ? 'bg-red-50 text-red-600' : message.type === 'success' ? 'bg-green-50 text-green-600' : 'bg-gray-50 text-gray-600']">
        <p>{{ message.text }}</p>
        <ul v-if="message.errors.length" class="list-disc pl-3 mt-3">
          <li v-for="(error, index) in message.errors" :key="index">
            {{ error }}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from '@nuxtjs/composition-api';
import InputRadioGroup from '@/components/atoms/InputRadioGroup';
import InputDatePicker from '@/components/atoms/InputDatePicker';
import moment from 'moment';
export default {
  name: 'IndexPage',

  components: {
    InputRadioGroup,
    InputDatePicker
  },

  setup() {
    const formData = ref({})
    const formErrors = ref({})
    const message = ref({
      type: '',
      text: '',
      errors: []
    })
    // Objects data
    const servers = ref({
      selected: null,
      options: [
        {
          value: 1,
          label: 'Hobby',
          description: '8GB / 4 CPUs · 160 GB SSD disk',
          price: "12",
        },
        {
          value: 2,
          label: 'Startup',
          description: '12GB / 6 CPUs · 256 GB SSD disk',
          price: "22",
        },
        {
          value: 3,
          label: 'Business',
          description: '16GB / 8 CPUs · 512 GB SSD disk',
          price: "40",
        },
        {
          value: 4,
          label: 'Enterprise',
          description: '32GB / 12 CPUs · 1024 GB SSD disk',
          price: "65",
        },
      ]
    })
    const startDate = ref({
      selected: new Date(),
      options: {
        format: 'DD-MM-YYYY',
        minDate: moment().toDate(),
        maxDate: moment().add(1, 'years').toDate()
      }
    })
    const endDate = ref({
      selected: null,
      options: {
        format: 'DD-MM-YYYY',
        minDate: moment(new Date(startDate.value.selected)).add(1, 'days').toDate(),
        maxDate: moment(new Date(startDate.value.selected)).add(1, 'years').toDate()
      }
    })

    // Make sure the end date's options and value are still valid based on changes made to the start date
    const onStartDateBlur = () => {
      endDate.value.options.minDate = moment(new Date(startDate.value.selected)).add(1, 'days').toDate()
      endDate.value.options.maxDate = moment(new Date(startDate.value.selected)).add(1, 'years').toDate()
      if(endDate.value.selected < endDate.value.options.minDate) {
        endDate.value.selected = endDate.value.options.minDate;
      }
    }

    const checkAvailability = async () => {
      // Reset errors and message
      formErrors.value = {}
      message.value.type = ''
      message.value.text = ''
      message.value.errors = []

      // Validate and prepare fields
      if(servers.value.selected) {
        formData.value.serverId = servers.value.selected
      } else {
        formErrors.value.serverId = []
        formErrors.value.serverId.push("Selecteer alsjeblieft een server")
        message.value.errors.push('Selecteer alsjeblieft een server')
      }

      if(startDate.value.selected) {
        formData.value.startDate = startDate.value.selected
      } else {
        formErrors.value.startDate = []
        formErrors.value.startDate.push("Selecteer alsjeblieft een startdatum")
        message.value.errors.push('Selecteer alsjeblieft een startdatum')
      }

      if(endDate.value.selected) {
        formData.value.endDate = endDate.value.selected
      } else {
        formErrors.value.endDate = []
        formErrors.value.endDate.push("Selecteer alsjeblieft een einddatum")
        message.value.errors.push('Selecteer alsjeblieft een einddatum')
      }

      if(Object.keys(formErrors.value).length) {
        message.value.type = 'error'
        message.value.text = 'Het formulier bevat fouten:'
      }

      const isValid = !Object.keys(formErrors.value).length;

      if(isValid) {
        try {
          // Placeholder request
          const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
            method: 'POST',
            body: JSON.stringify({
              title: 'foo',
              body: 'bar',
              userId: 1,
            }),
            headers: {
              'Content-type': 'application/json; charset=UTF-8',
            },
          })

          // json containing the response data
          const json = await response.json();

          // Simulated availability response
          if(formData.value.serverId === 4) { // response (json) body returns null
            message.value.type = 'success'
            message.value.text = `De server is nog beschikbaar tussen ${moment(new Date(startDate.value.selected)).format('DD-MM-YYYY')} en ${moment(new Date(endDate.value.selected)).format('DD-MM-YYYY')}!`
          } else { // response (json) body returns date
            // Calculate difference in days
            const diff = Math.ceil(Math.abs(formData.value.startDate - formData.value.endDate) / (1000 * 60 * 60 * 24));
            const date = moment(new Date(startDate.value.selected)).add(Math.floor(Math.random() * diff), 'days').format('DD-MM-YYYY');

            message.value.type = 'error'
            // Returns random date between the given start and end date
            message.value.text = 'De server is helaas al gereserveerd voor ' + date;
          }
        } catch (error) {
          message.value.type = 'error'
          message.value.text = error
        }
      }
    }
    return {
      servers,
      startDate,
      endDate,
      formErrors,
      message,
      onStartDateBlur,
      checkAvailability
    }
  }
}
</script>
