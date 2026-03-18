<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-50">
    <div class="text-center">
      <h1 class="text-2xl font-bold text-gray-900 mb-4">Welcome</h1>
      <p class="text-gray-600">Redirecting...</p>
    </div>
  </div>
</template>

<script setup>
const user = useSupabaseUser()

watchEffect(() => {
  if (user.value) {
    navigateTo('/account')
  } else {
    navigateTo('/login')
  }
})
</script>
