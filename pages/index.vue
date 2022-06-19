<template>
  <div class="h-screen flex flex-col justify-center py-12 sm:px-6 lg:px-8 bg-gray-100">
    <div class="sm:mx-auto sm:w-full sm:max-w-md text-center">
      <span class="text-sm text-gray-400">Opdracht 1</span>
      <h2 class="text-center text-3xl font-extrabold text-gray-900">Form Picker</h2>
    </div>
    <div class="mt-8 sm:mx-auto sm:w-full sm:max-w-md">
      <CheckAvailabilityForm />
    </div>
  </div>
</template>

<script>
import CheckAvailabilityForm from "@/components/molecules/CheckAvailabilityForm";
export default {
  name: 'IndexPage',
  components: {
    CheckAvailabilityForm
  }
}
</script>
