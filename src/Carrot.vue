<template>
  <form>
    <input
      type="text"
      v-model="form.id"
    />
    <input
      type="text"
      v-model="form.name"
    />
    <input
      type="text"
      v-model="form.email"
    />
    <button
      @click="handleAuth"
      type="button"
    >
      Submit
    </button>
  </form>
</template>

<script setup>
import { onMounted, reactive } from "vue"
import CryptoJS from "crypto-js"

const form = reactive({
  id: "",
  name: "Test",
  email: "purelyugly@gmail.com"
})

const handleAuth = () => {
  authCarrot()
}

const authCarrot = () => {
  const userId = form.id
  const userAuthKey = import.meta.env.VITE_USER_AUTH_KEY

  const hash = CryptoJS.HmacSHA256(userId, userAuthKey)
  carrotquest.auth(userId, hash)

  // carrotquest.identify([
  //   {
  //     name: form.name,
  //     email: form.email
  //   }
  // ])
}

// onMounted(() => {
//   authCarrot()
// })
</script>
