<template>
    <Head title="Users Index" />
  
    <AdminAuthenticatedLayout>
      <template #header>
        <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">Users Index</h2>
      </template>
  
      <div class="py-12">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
          <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
            <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
              <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                  <tr>
                    <th scope="col" class="px-6 py-3">ID</th>
                    <th scope="col" class="px-6 py-3">Username</th>
                    <th scope="col" class="px-6 py-3">Email</th>
                    
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="user in users" :key="user.id">
                    <td class="px-6 py-4 border-b">{{ user.id }}</td>
                    <td class="px-6 py-4 border-b">{{ user.name }}</td>
                    <td class="px-6 py-4 border-b">{{ user.email }}</td>
                    
                  </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </AdminAuthenticatedLayout>
  </template>
  
  <script>
  import { defineComponent, computed } from 'vue';
  import { usePage, Head } from '@inertiajs/vue3';
  import AdminAuthenticatedLayout from '@/Layouts/AdminAuthenticatedLayout.vue';
  
  export default defineComponent({
    components: {
      AdminAuthenticatedLayout,
      Head,
    },
    setup() {
      const users = computed(() => usePage().props.users);
      return { users };
    },
  });
  </script>
  
  <style scoped>
  /* Add any custom styles here */
  </style>
  