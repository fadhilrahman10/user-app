<script setup>
import GuestLayout from '@/Layouts/GuestLayout.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import axios from "axios";
import {ref} from "vue";

const form = useForm({
    name: '',
    email: '',
    dob: '',
    identity_number: ''
});

const alert = ref(false);

const submit = async () => {
    await axios.post('/api/register', {
        name: form.name,
        email: form.email,
        dob: form.dob,
        identity_number: form.identity_number,
    })
        .then(response => {
            console.log(response)
            form.reset()
            alert.value = true;
        })
        .catch(error => {
            const errors = error.response.data.errors;

            for (const key in errors) {
                if (errors.hasOwnProperty(key)) {
                    form.setError(key, errors[key][0]);
                }
            }
        })
};
</script>

<template>
    <GuestLayout>
        <Head title="Register" />

        <div v-if="alert" class="p-4 mb-4 text-sm text-green-800 rounded-lg bg-green-50" role="alert">
            <span class="font-medium">Success!</span> Create new User
        </div>

        <form @submit.prevent="submit">
            <div>
                <InputLabel for="name" value="Name" />

                <TextInput
                    id="name"
                    type="text"
                    class="mt-1 block w-full"
                    v-model="form.name"
                    autofocus
                    autocomplete="name"
                />

                <InputError class="mt-2" :message="form.errors.name" />
            </div>

            <div class="mt-4">
                <InputLabel for="email" value="Email" />

                <TextInput
                    id="email"
                    type="email"
                    class="mt-1 block w-full"
                    v-model="form.email"
                    autocomplete="email"
                />

                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <div class="mt-4">
                <InputLabel for="dob" value="Date of Birth" />

                <TextInput
                    id="dob"
                    type="date"
                    class="mt-1 block w-full"
                    v-model="form.dob"
                    autocomplete="dob"
                />

                <InputError class="mt-2" :message="form.errors.dob" />
            </div>

            <div class="mt-4">
                <InputLabel for="identity_number" value="Identity Number" />

                <TextInput
                    id="identity_number"
                    type="text"
                    class="mt-1 block w-full"
                    v-model="form.identity_number"
                    autocomplete="identity_number"
                />

                <InputError class="mt-2" :message="form.errors.dob" />
            </div>

            <div class="flex items-center justify-end mt-4">
                <PrimaryButton class="ms-4" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                    Submit
                </PrimaryButton>
            </div>
        </form>
    </GuestLayout>
</template>
