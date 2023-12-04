<template>
  <div role="alert" class="alert alert-error" v-if="errorMessage">
          <Icon name="ph:warning" class="mr-2" />
          <span>{{ errorMessage }}</span>
        </div>
  <form @submit.prevent="updateProfile">
    <div class="flex flex-col items-start text-lg mb-6 md:mb-8">
      <label for="username" class="w-20">Username</label>
      <input type="text" id="username" placeholder="Username" class="input input-bordered w-full" v-model="username"/>
    </div>
    <div class="flex flex-col items-start text-lg mb-6 md:mb-8">
      <label for="avatar_url" class="w-20">Avatar</label>
      <input type="text" id="avatar_url" placeholder="Avatar URL" class="input input-bordered w-full" v-model="avatar_url"/>
    </div>
    <div>
        <input
          type="submit"
          class="button primary block"
          :value="loading ? 'Loading ...' : 'Update'"
          :disabled="loading"
        />
      </div>
  </form>

</template>

<script setup>
  definePageMeta({
    title: 'Profile',
    description: 'Profile Page',
    layout: 'default',
    middleware: ['auth']
  })
  const client = useSupabaseClient();
  const user = useSupabaseUser();
  const loading = ref(true);
  const username = ref('');
  const avatar_url = ref('');
  const errorMessage = ref(null);

  loading.value = true;

  const { data } = await client.from('profiles').select().eq('id', user.value.id).single();

  if (data) {
    username.value = data.username;
    avatar_url.value = data.avatar_url;
  }

  loading.value = false;

  async function updateProfile() {
    try {
      loading.value = true;
      const user = useSupabaseUser();

      const updates = {
        id: user.value.id,
        username: username.value,
        updated_at: new Date(),
      }

      const { error } = await client.from('profiles').upsert(updates, {
        returning: 'minimal'
      });

      if (error) throw error;
    } catch(error) {
      errorMessage.value = error.message
    } finally {
      loading.value = false;
    }
  }
</script>