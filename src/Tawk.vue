<template>
  <button
    type="button"
    @click="handleAuth()"
    v-if="!tawkInit"
  >
    Авторизация
  </button>
</template>

<script setup>
import { onMounted, ref } from "vue"

const tawkInit = ref(false)

const initializeTawk = () => {
  window.Tawk_API = window.Tawk_API || {}
  window.Tawk_LoadStart = new Date()

  const propertyId = import.meta.env.VITE_PROPERTY_ID
  const widgetId = import.meta.env.VITE_WIDGET_ID

  const script = document.createElement("script")
  const first_script_tag = document.getElementsByTagName("script")[0]

  script.async = true
  script.src = `https://embed.tawk.to/${propertyId}/${widgetId}`
  script.setAttribute("crossorigin", "*")

  first_script_tag.parentNode.insertBefore(script, first_script_tag)

  tawkInit.value = true
}

const handleAuth = () => {
  initializeTawk()
}

// onMounted(() => {
//   initializeTawk()
// })
</script>
