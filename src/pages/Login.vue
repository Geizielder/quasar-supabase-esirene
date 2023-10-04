<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handlerLogin">
      <p class="col-12 text-h5 text-center">Login</p>
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-y-md">
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
          :rules="[ val => (val && val.length > 0) || 'Password é obrigatório']"
          type="password"
        />

       <div class="full-width  q-pt-md">
        <q-btn
          label="Login"
          color="primary"
          class="full-width q-pa-md"
          type="submit"
          outline
        />
       </div>
       <div class="full-width q-gutter-y-sm">
        <q-btn
          label="Cadastrar"
          color="primary"
          class="full-width"
          to="/cadastrar"
          flat
          size="sm"
        />
        <q-btn
          label="Recuperar Senha ?"
          color="primary"
          class="full-width"
          :to="{ name: 'forgot-password' }"
          flat
          size="sm"
        />
       </div>
      </div>
    </q-form>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue'
import useAuthUser from 'src/composables/UseAuthUser'
import useNotify from 'src/composables/UseNotify'
import { useRouter } from 'vue-router'

export default defineComponent({
  name: 'PageLogin',

  setup () {
    const router = useRouter()
    const { login, isLoggedIn } = useAuthUser()
    const { notifyError, notifySuccess } = useNotify()
    const form = ref({
      email: '',
      password: ''
    })

    onMounted(() => {
      if (isLoggedIn) {
        router.push({ name: 'me' })
      }
    })

    const handlerLogin = async () => {
      try {
        await login(form.value)
        notifySuccess('Logado com Sucesso')
        router.push({ name: 'me' })
      } catch (error) {
        notifyError(error.message)
        // alert(error.message)
      }
    }
    return {
      form,
      handlerLogin
    }
  }
})
</script>
