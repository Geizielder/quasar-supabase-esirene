<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handlePasswordReset">
      <p class="col-12 text-h5 text-center">Recuperar Senha</p>
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-y-md">

        <q-input
          label="New Password"
          v-model="password"
          lazy-rules
          :rules="[ val => (val && val.length >= 6) || 'Password é obrigatório e deve ter minimo de 6 caracteres']"
          type="password"
        />

       <div class="full-width  q-pt-md q-gutter-sm">
        <q-btn
          label="Enviar"
          color="primary"
          class="full-width q-pa-md"
          outline
          type="submit"
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
  name: 'PageResetPassword',

  setup () {
    const { resetPassword } = useAuthUser()
    const { notifyError, notifySuccess } = useNotify()
    const router = useRouter()

    const password = ref('')

    const handlePasswordReset = async () => {
      try {
        await resetPassword(password.value)
        notifySuccess()
        router.push({ name: 'login' })
      } catch (error) {
        notifyError(error.message)
      }
    }

    return {
      password,
      handlePasswordReset
    }
  }
})

</script>
