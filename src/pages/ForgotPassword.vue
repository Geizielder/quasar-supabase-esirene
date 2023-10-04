<template>
  <q-page padding>
    <q-form class="row justify-center" @submit.prevent="handleForgotPassword">
      <p class="col-12 text-h5 text-center">Recuperar Senha</p>
      <div class="col-md-4 col-sm-6 col-xs-10 q-gutter-y-md">

        <q-input
          label="Email"
          v-model="email"
          lazy-rules
          :rules="[ val => (val && val.length > 0) || 'E-mail é obrigatório']"
          type="email"
        />

       <div class="full-width  q-pt-md q-gutter-sm">
        <q-btn
          label="Enviar"
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
          size="sm"
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

export default defineComponent({
  setup () {
    const { sendPasswordRestEmail } = useAuthUser()
    const { notifyError, notifySuccess } = useNotify()

    const email = ref('')

    const handleForgotPassword = async () => {
      try {
        await sendPasswordRestEmail(email.value)
        notifySuccess(`E-mail de redefinição de senha enviado para: ${email.value}`)
      } catch (error) {
        notifyError(error.message)
      }
    }

    return {
      email,
      handleForgotPassword
    }
  }
})
</script>
