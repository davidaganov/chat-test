<template>
  <div class="container">
    <form
      class="form"
      v-if="!auth"
    >
      <input
        class="form__input"
        type="text"
        name="name"
        placeholder="Введите имя"
        v-model="form.name"
      />
      <input
        class="form__input"
        type="text"
        name="email"
        placeholder="Введите email"
        v-model="form.email"
      />
      <button
        type="button"
        class="form__button"
        @click="handleAuth"
      >
        Submit
      </button>
    </form>

    <div
      class="user"
      v-else
    >
      <p class="user__name">{{ form.name }}</p>
      <p class="user__email">{{ form.email }}</p>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, reactive } from "vue"
import CryptoJS from "crypto-js"

const auth = ref(false)
const form = reactive({
  name: "",
  email: ""
})

const handleAuth = () => {
  authCarrot()
}

const authCarrot = () => {
  const date = new Date()
  const RegDate = date.toISOString()

  const userId = form.name
  const userAuthKey = import.meta.env.VITE_USER_AUTH_KEY

  const hash = CryptoJS.HmacSHA256(userId, userAuthKey)
  carrotquest.auth(userId, hash)

  carrotquest.identify({
    $name: form.name,
    $email: form.email,
    RegDate
  })

  auth.value = true
}

onMounted(() => {
  localStorage.removeItem("carrotquest_last_activity")
  localStorage.removeItem("carrotquest_requests")
  localStorage.removeItem("carrotquest_data")

  const cookies = document.cookie.split(";")
  for (let i = 0; i < cookies.length; i++) {
    const cookie = cookies[i]
    const eqPos = cookie.indexOf("=")
    const name = eqPos > -1 ? cookie.substr(0, eqPos) : cookie
    document.cookie = name + "=;expires=Thu, 01 Jan 1970 00:00:00 GMT;"
    document.cookie = name + "=; path=/; expires=Thu, 01 Jan 1970 00:00:01 GMT;"
  }
})
</script>

<style>
body {
  margin: 0;
  padding: 0;
}

.container {
  display: grid;
  place-items: center;
  height: 100vh;
  width: 100%;
}

.form {
  display: flex;
  flex-direction: column;
  gap: 15px;
  max-width: 400px;
  width: 100%;
}

.form__input {
  padding: 8px 10px;
  font-family: "Roboto", sans-serif;
  font-size: 18px;
  color: #070707;
  border: 1px solid #b1b1b1;
  border-radius: 10px;
}

.form__button {
  padding: 8px 10px;
  background-color: #004a53;
  border-radius: 10px;
  border: none;
  color: white;
  font-family: "Roboto", sans-serif;
  font-size: 18px;
  cursor: pointer;
}

.user {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.user__name {
  padding: 0;
  font-family: "Roboto", sans-serif;
  font-size: 24px;
  color: #004a53;
}

.user__email {
  padding: 0;
  font-family: "Roboto", sans-serif;
  font-size: 24px;
  color: #004a53;
}
</style>
