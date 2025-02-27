<script setup>
import { ref } from 'vue';
import { Link, useForm } from '@inertiajs/vue3';
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import Swal from 'sweetalert2';

const props = defineProps({
    medicines: Array
});

const form = useForm({
  medicine_id: null,
  name: '',
  lprice: 0,
  mprice: 0,
  hprice: 0,
  quantity: 1,
  dosage: '',
  expdate: '',
});

function handleAddToCart(medicine) {
  Swal.fire({
    title: 'Add to Cart',
    text: `Are you sure you want to add ${medicine.name} (Quantity: ${medicine.quantity}) to your cart?`,
    icon: 'question',
    showCancelButton: true,
    confirmButtonText: 'Add to Cart',
    confirmButtonColor: '#28a745',
    cancelButtonColor: 'gray',
    cancelButtonText: 'Cancel'
  }).then((result) => {
    if (result.isConfirmed) {
      // Set the form data based on the selected medicine
      form.medicine_id = medicine.id;
      form.name = medicine.name;
      form.lprice = medicine.lprice;
      form.mprice = medicine.mprice;
      form.hprice = medicine.hprice;
      form.quantity = medicine.quantity;
      form.dosage = medicine.dosage;
      form.expdate = medicine.expdate;

      form.post(route('cart.store'), {
        onSuccess: () => {
          Swal.fire({
            icon: 'success',
            title: 'Added!',
            text: 'Medicine added to your cart successfully.',
            timer: 3000,
            showConfirmButton: false,
            toast: true,
            position: 'top-end',
            background: '#B5C99A',
            color: '#000'
          });
        },
      });
    }
  });
}

const showingNavigationDropdown = ref(false);
</script>


<template>
    <AuthenticatedLayout>
        <Head title="Medicines" />

        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 dark:text-gray-200 leading-tight">List of Medicines</h2>
        </template>
        <main>
            <div class="py-12">
                <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                    <div class="relative overflow-x-auto shadow-md sm:rounded-lg">
                        <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
                            <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                                <tr>
                                    <th scope="col" class="px-6 py-3">Name</th>
                                    <th scope="col" class="px-6 py-3">Low Price</th>
                                    <th scope="col" class="px-6 py-3">Median Price</th>
                                    <th scope="col" class="px-6 py-3">Highest Price</th>
                                    <th scope="col" class="px-6 py-3">Quantity</th>
                                    <th scope="col" class="px-6 py-3">Dosage</th>
                                    <th scope="col" class="px-6 py-3">Exp Date</th>
                                    <th scope="col" class="px-6 py-3">Reserve</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr
                                    v-for="medicine in medicines"
                                    :key="medicine.id"
                                    class="odd:bg-white odd:dark:bg-gray-900 even:bg-gray-50 even:dark:bg-gray-800 border-b dark:border-gray-700"
                                >
                                    <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">{{ medicine.name }}</th>
                                    <td class="px-6 py-4">₱{{ medicine.lprice }}</td>
                                    <td class="px-6 py-4">₱{{ medicine.mprice }}</td>
                                    <td class="px-6 py-4">₱{{ medicine.hprice }}</td>
                                    <td class="px-6 py-4">
                                        <!-- Quantity Selector -->
                                        <input
                                            type="number"
                                            v-model.number="medicine.quantity"
                                            min="1"
                                            class="w-20 p-1 border border-gray-300 rounded dark:bg-gray-800 dark:text-white"
                                        />
                                    </td>
                                    <td class="px-6 py-4">{{ medicine.dosage }}</td>
                                    <td class="px-6 py-4">{{ medicine.expdate }}</td>
                                    <td class="flex px-6 py-4">
                                        <Link
                                            class="px-4 py-2 ml-4 bg-[#28a745] hover:bg-[#D2E0BE] text-white rounded"
                                            @click.prevent="handleAddToCart(medicine)"
                                            href="#"
                                        >
                                            Add
                                        </Link>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </main>
    </AuthenticatedLayout>
</template>
