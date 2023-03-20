<script setup lang="ts">
import axios from "axios";

axios.defaults.withCredentials = true;
axios.defaults.headers.common["Access-Control-Allow-Origin"] = true;

definePageMeta({
  layout: "centered",
});

interface LoginPayload {
  email: string;
  password: string;
}

const form = ref({
  email: "",
  password: "",
});

const user = ref({});

async function login(payload: LoginPayload) {
  await axios.get("http://localhost/sanctum/csrf-cookie");
  await axios.post("http://localhost/login", payload);

  let { data} = await axios.get("http://localhost/user");
  user.value = data;
}



</script>
<template>
  <div class="login">
    <pre>
      {{ user }}
    </pre>
    <h1>Login</h1>
    <form @submit.prevent="login(form)">
      <label>
        <div>Email</div>
        <input type="text" v-model="form.email" />
      </label>

      <label>
        <div>Password</div>
        <input type="password" v-model="form.password" />
      </label>
      <button class="btn">Login</button>
    </form>

    <p>
      Don't have an account?
      <NuxtLink class="underline text-lime-600" to="/register"
      >Register now!</NuxtLink
      >
    </p>
  </div>
</template>
