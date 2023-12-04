<template>
  <div class="h-screen">
    <form class="flex flex-col justify-center items-center h-full" @submit.prevent="register">
      <div class="max-w-xl h-56 bg-white dark:bg-gray-500 shadow-xl p-4">
        <div class="flex flex-col">
          <label for="email">Email</label>
          <input type="email" id="email" v-model="email" />
        </div>
        <div class="flex flex-col">
          <label for="password">Password</label>
          <input type="password" id="password" v-model="password" />
        </div>
        <div class="flex flex-col">
          <button class="btn" type="submit">Register</button>
        </div>
        <div class="flex flex-col">
          <div role="alert" class="alert alert-error" v-if="errorMessage">
            <Icon name="ph:warning" class="mr-2" />
            <span>{{ errorMessage }}</span>
          </div>
          <div role="alert" class="alert alert-success" v-if="successMessage">
            <Icon name="ph:check-circle" class="mr-2" />
            <span>{{ successMessage }}</span>
          </div>
        </div>
        <div>
          <p>Already have an account? <NuxtLink to="/login">Login</NuxtLink></p>
        </div>
      </div>
    </form>
  </div>

</template>

<script setup lang="ts">
  const client = useSupabaseClient()
  const email = ref('');
  const password = ref(null);
  const errorMessage = ref(null);
  const successMessage = ref(null);

  definePageMeta({
    title: 'Register',
    description: 'Register page',
    layout: 'authentication'
  })

  async function register() {
    try {
      const { data, error } = await client.auth.signUp({
        email: email.value,
        password: password.value
      })
      if (error) throw error;
      successMessage.value = 'Check your email for the confirmation link'
    } catch (error) {
      errorMessage.value = error.message
    }
  }

</script>