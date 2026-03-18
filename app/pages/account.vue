<template>
  <div class="min-h-screen bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-3xl mx-auto">
      <div class="bg-white shadow overflow-hidden sm:rounded-lg">
        <div class="px-4 py-5 sm:px-6 flex justify-between items-center">
          <div>
            <h3 class="text-lg leading-6 font-medium text-gray-900">
              Account Information
            </h3>
            <p class="mt-1 max-w-2xl text-sm text-gray-500">
              Your personal details and account settings.
            </p>
          </div>
          <button
            @click="handleLogout"
            :disabled="loading"
            class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md text-white bg-red-600 hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-red-500 disabled:opacity-50"
          >
            {{ loading ? "Logging out..." : "Logout" }}
          </button>
        </div>
        <div class="border-t border-gray-200">
          <dl>
            <div
              class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6"
            >
              <dt class="text-sm font-medium text-gray-500">Email</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                {{ user?.email || "N/A" }}
              </dd>
            </div>
            <div
              class="bg-white px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6"
            >
              <dt class="text-sm font-medium text-gray-500">User ID</dt>
              <dd
                class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2 font-mono"
              >
                {{ user?.id || "N/A" }}
              </dd>
            </div>
            <div
              class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6"
            >
              <dt class="text-sm font-medium text-gray-500">Created At</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                {{ formatDate(user?.created_at) }}
              </dd>
            </div>
            <div
              class="bg-white px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6"
            >
              <dt class="text-sm font-medium text-gray-500">Last Sign In</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                {{ formatDate(user?.last_sign_in_at) }}
              </dd>
            </div>
            <div
              class="bg-gray-50 px-4 py-5 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-6"
            >
              <dt class="text-sm font-medium text-gray-500">Email Confirmed</dt>
              <dd class="mt-1 text-sm text-gray-900 sm:mt-0 sm:col-span-2">
                <span
                  :class="
                    user?.email_confirmed_at
                      ? 'text-green-600'
                      : 'text-yellow-600'
                  "
                >
                  {{ user?.email_confirmed_at ? "Yes" : "No" }}
                </span>
              </dd>
            </div>
          </dl>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient();
const user = useSupabaseUser();
const loading = ref(false);

const formatDate = (dateString) => {
  if (!dateString) return "N/A";
  return new Date(dateString).toLocaleString();
};

const handleLogout = async () => {
  try {
    loading.value = true;
    await supabase.auth.signOut();
    navigateTo("/login");
  } catch (error) {
    console.error("Error logging out:", error);
  } finally {
    loading.value = false;
  }
};
</script>
