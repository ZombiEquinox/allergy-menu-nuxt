<template>
  <div class="bg-primary h-screen overflow-hidden flex items-center justify-center">
    <div class="bg-white lg:w-5/12 md:6/12 w-10/12 shadow-3xl">
      <div>
        <h2 class="text-3xl font-bold text-center text-primary p-12">Login</h2>
      </div>
      <div role="alert" class="alert alert-error" v-if="errorMessage">
        <Icon name="ph:warning" class="mr-2" />
        <span>{{ errorMessage }}</span>
      </div>
      <div class="px-12 pb-12 md:px-24 md:pb-24">
        <form @submit.prevent="signIn">
          <div class="flex flex-col items-start text-lg mb-6 md:mb-8">
            <label for="email" class="w-20">Email</label>
            <input type="text" id="email" placeholder="Email" class="input input-bordered w-full" v-model="email"/>
          </div>
          <div class="flex flex-col items-start text-lg mb-6 md:mb-8">
            <label for="password" class="w-20">Password</label>
            <input type="password" id="password" placeholder="Password" class="input input-bordered w-full" v-model="password"/>
          </div>
          <button class="btn btn-block">Login</button>
        </form>
        <p>Dont have an account? <NuxtLink to="/register">Signup</NuxtLink></p>
      </div>
    </div>
   </div>
</template>

<script setup lang="ts">
definePageMeta({
  title: 'Login',
  description: 'Login page',
  layout: 'authentication'
})
const client = useSupabaseClient()
const router = useRouter();

const email = ref('');
const password = ref(null);
const errorMessage = ref(null);

async function signIn() {
  try {
    const { error } = await client.auth.signInWithPassword({
      email: email.value,
      password: password.value
    })
    if (error) throw error;
    await navigateTo('/dashboard');
  } catch (error) {
    errorMessage.value = error.message
  }
}

</script>
```