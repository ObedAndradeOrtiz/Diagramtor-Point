<script setup>
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
defineProps({
    canResetPassword: Boolean,
    status: String,
});
const form = useForm({
    email: '',
    password: '',
    remember: false
});
const submit = () => {
    form.post(route('login'), {
        onFinish: () => form.reset('password'),
    });
};
</script>
<template>
  <GuestLayout>
    <Head title="Inicio de sesion"/>
    <div class="flex flex-col overflow-y-auto ">
      <div class="flex items-center justify-center p-6 sm:p-12">
        <div class="w-full">
          <h1 class="mb-4 text-xl font-semibold text-gray-700">Inicio de sesion</h1>
          <div v-if="status" class="mb-4 text-sm font-medium text-green-600">
            {{ status }}
          </div>
          <form @submit.prevent="submit">
            <div class="mt-4">
              <InputLabel for="email" value="Correo electronico"/>
              <TextInput id="email" type="email" class="block w-full mt-1" v-model="form.email" required autofocus autocomplete="username" />
              <InputError class="mt-2" :message="form.errors.email" />
            </div>
            <div class="mt-4">
              <InputLabel for="password" value="Contraseña"/>
              <TextInput id="password" type="password" class="block w-full mt-1" v-model="form.password" required autocomplete="current-password"/>
              <InputError class="mt-2" :message="form.errors.password" />
            </div>
            <div class="flex items-center justify-end mt-4">
              <PrimaryButton class="ml-4" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                Ingresar
              </PrimaryButton>
            </div>
          </form>
        </div>
      </div>
    </div>
  </GuestLayout>
</template>


