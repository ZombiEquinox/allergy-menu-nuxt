<template>
  <div class="navbar bg-base-100">
    <div class="flex-1">
      <a class="btn btn-ghost text-xl">Allergy Menu</a>
    </div>
    <div class="flex-none">
      
      <div class="dropdown dropdown-end" v-if="user">
        <UiUserAvatar/>
        <ul tabindex="0" class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-base-100 rounded-box w-52">
          <li>
            {{ name }}
          </li>
          <li>
            <NuxtLink to="#">
              Profile
            </NuxtLink>
          </li>
          <li>
            <NuxtLink to="#">Settings</NuxtLink>
          </li>
          <li>
            <a @click="logout">Logout</a>
          </li>
        </ul>
      </div>
      <div v-else>
          <NuxtLink to="/login" class="btn btn-ghost">Login</NuxtLink>
      </div>
      <div v-if="user">
        <NuxtLink to="/dashboard" class="btn btn-ghost">Dashboard</NuxtLink>
      </div>
      <div class="mx-4">
        <label class="swap swap-rotate">
          <input type="checkbox" class="theme-controller" value="synthwave" />
          <Icon name="ph:sun-fill" class="swap-on"/>
          <Icon name="ph:moon-fill" class="swap-off"/>
        </label>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
  const supabase = useSupabaseClient();
  const user = useSupabaseUser();

const logout = async () => {
  const { error } = await supabase.auth.signOut();

  if (error) {
    console.error(error);
    return;
  }

  await navigateTo('/login');
};

const name = computed(
  () => user.value?.user_metadata.full_name ?? user.value?.email
);

const avatar = computed(
  () => user.value?.user_metadata.avatar_url
);
</script>