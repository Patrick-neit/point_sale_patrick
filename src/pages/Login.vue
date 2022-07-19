<template>
    <form action class="form" @submit.prevent="login">
        <div class="surface-0 flex align-items-center justify-content-center min-h-screen min-w-screen overflow-hidden">
            <div class="grid justify-content-center p-2 lg:p-0" style="min-width:80%">
                <div class="col-12 mt-5 xl:mt-0 text-center">
                    <img :src="'layout/images/logo-' + logoColor + '.svg'" alt="Sakai logo" class="mb-5" style="width:81px; height:60px;">
                </div>
                <div class="col-12 xl:col-6" style="border-radius:56px; padding:0.3rem; background: linear-gradient(180deg, var(--primary-color), rgba(33, 150, 243, 0) 30%);">
                    <div class="h-full w-full m-0 py-7 px-4" style="border-radius:53px; background: linear-gradient(180deg, var(--surface-50) 38.9%, var(--surface-0));">
                        <div class="text-center mb-5">
                            <img src="layout/images/avatar.png" alt="Image" height="50" class="mb-3">
                            <div class="text-900 text-3xl font-medium mb-3">Bienvenido de Vuelta !</div>
                            <span class="text-600 font-medium">Comienza ahora..</span>
                        </div>
                    
                        <div class="w-full md:w-10 mx-auto">
                        
                            <InputText id="email" v-model="email" type="text" class="w-full mb-3" placeholder="Email" style="padding:1rem;" />

                            <label for="codigo" class="block text-900 font-medium text-xl mb-2">Codigo Usuario*</label>
                            <input v-model="codigo" class="form-input w-full mb-3" type="number" id="codigo" style="padding:1rem;" placeholder="Ingrese Codigo" >
                    
                    
                            <!-- <Password id="codigo" v-model="password" placeholder="Ingrese su Codigo" :toggleMask="true" class="w-full mb-3" inputClass="w-full" inputStyle="padding:1rem"></Password> -->
                    
                            <div class="flex align-items-center justify-content-between mb-5">
                                <div class="flex align-items-center">
                                    <Checkbox id="rememberme1" v-model="checked" :binary="true" class="mr-2"></Checkbox>
                                    <label for="rememberme1">Recuerdame</label>
                                </div>
                                <a class="font-medium no-underline ml-2 text-right cursor-pointer" style="color: var(--blue-color)">Olvidaste tu contraseña?</a>
                            </div>
                            <Button class="form-submit w-full p-3 text-xl" label="Ingresa"  type="submit" value="login"></button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </form>
</template>

<script>
export default {
    data: () => ({
        email: "",
        codigo:"",
        error: false

    }),
    methods:{
        login(){
            console.log(this.email),
            console.log(this.codigo)
        }
    },

    computed: {
        logoColor() {
            if (this.$appState.darkTheme) return 'white';
            return 'dark';
        }
    }
}
</script>

<style scoped>
.pi-eye {
    transform:scale(1.6);
    margin-right: 1rem;
}

.pi-eye-slash {
    transform:scale(1.6);
    margin-right: 1rem;
}
</style>