<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handleRegister">
      <p class="col-12 text-h5 text-center">Cadastrar</p>
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-y-md">
        <q-input
          label="Nome"
          v-model="form.name"
          lazy-rules
          :rules="[ val => (val && val.length > 0) || 'Nome é obrigatório']"
        />
        <q-input
          label="Celular"
          v-model="form.phone"
          mask="(##) ##### - ####"
          lazy-rules
          :rules="[ val => (val && val.length > 0 || val.length == 11) || 'Celular é obrigatório']"
        />
        <q-input
          label="Email"
          v-model="form.email"
          lazy-rules
          :rules="[ val => (val && val.length > 0) || 'E-mail é obrigatório']"
          type="email"
        />
        <q-input
          label="Password"
          v-model="form.password"
          lazy-rules
          :rules="[ val => (val && val.length >= 6) || 'Password é obrigatório e deve ter minimo de 6 caracteres']"
          type="password"
        />

       <div class="full-width  q-pt-md">
        <q-btn
          label="Cadastrar"
          color="primary"
          class="full-width q-pa-md"
          type="submit"
          outline
        />

        <q-btn
          label="Voltar"
          color="dark"
          class="full-width"
          flat
          :to="{ name: 'login' }"
        />
       </div>
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'
import useAuthUser from 'src/composables/UseAuthUser'
import useNotify from 'src/composables/UseNotify'
import { useRouter } from 'vue-router'

export default defineComponent({
  name: 'PageRegister',
  setup () {
    const router = useRouter()
    const { register } = useAuthUser()
    const { notifyError, notifySuccess } = useNotify()
    const form = ref({
      name: '',
      phone: '',
      email: '',
      password: ''

    })
    const handleRegister = async () => {
      try {
        await register(form.value)
        notifySuccess('Cadastro realizado com sucesso.')
        router.push({
          name: 'email-confirmation',
          query: { email: form.value.email }
        })
      } catch (error) {
        notifyError(error)
      }
    }
    return {
      form,
      handleRegister
    }
  }
})
</script>
