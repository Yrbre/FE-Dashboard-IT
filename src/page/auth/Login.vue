<script setup lang="ts">
import router from '@/router';
import { useAuthStore } from '@/stores/auth.store';
import { Button, InputText, Message } from 'primevue';
import { ref } from 'vue';

const auth = useAuthStore();
const error = ref<string|null>(null);
const form = ref({
    email:'',
    password:''
});

async function login(){
    error.value = null;

if(!form.value.email || !form.value.password){
    error.value = "Email and Password required"
    return
}

try {
await auth.login(form.value.email, form.value.password);
router.push({name:'dashboard'});
}catch(e) {
    error.value = "Invalid email or password"
}


}
</script>

<template>
    <div class="flex items-center justify-center min-h-screen bg-surface-50 px-4 py-10">
        <div class="w-full max-w-100">
            <div class="bg-white p-8 rounded-2xl shadow-xl border border-surface-100">
                <!-- Header -->
                 <div class="text-center mb-8">
                    <div class="w-12 h-12 bg-primary-600 rounded-xl flex items-center justify-center text-white shadow-lg shadow-primary-200 mx-auto">
                        <i class="pi pi-bolt"></i>
                    </div>
                    <h1 class="text-2xl font-bold text-surface-900 mb-2">Welcome</h1>
                    <p class="text-surface-500">Please sign in to your account</p>
                 </div>
                 <form @submit.prevent="login" class="flex flex-col gap-5">
                    <Message 
                    v-if="error"
                    severity="error"
                    :closable="false">{{ error }}</Message>

                    <!-- email -->
                    <div class="flex flex-col gap-2">
                        <label for="email" class="font-medium text-surface-900">Email <span class="text-red-600">*</span></label>
                        <InputText id="email" v-model="form.email" type="text" placeholder="email@email.com"
                        fluid class="bg-surface-10 focus:bg-white"></InputText>
                    </div>
                    <!-- password -->
                    <div class="flex flex-col gap-2">
                        <label for="password" class="font-medium text-surface-900">Password<span class="text-red-600">*</span></label>
                        <InputText id="password" v-model="form.password" type="password" placeholder="********"
                        fluid class="bg-surface-10 focus:bg-white"></InputText>
                    </div>
                    
                    <Button
                    type="submit"
                    label="Sign In"
                    fluid
                    class="mt-2" />
            </form>
            
            </div>
            
        </div>
    </div>
</template>