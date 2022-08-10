<template>
  <v-row>
    <v-col class="mt-8">
      <v-card elevation="1">
        <v-card-title>
          Cadastro de Estudante
        </v-card-title>

        <form @submit.prevent="submit">
          <v-card-text>
            <v-text-field outlined v-model="name" :error-messages="nameErrors" :counter="80" label="Nome"
              placeholder="Informe o nome completo" required @input="$v.name.$touch()" @blur="$v.name.$touch()" />

            <v-text-field outlined v-model="email" :error-messages="emailErrors" :counter="80" label="E-mail"
              placeholder="Informe apenas um e-mail" required @input="$v.email.$touch()" @blur="$v.email.$touch()" />

            <v-text-field disabled outlined v-model="ra" :error-messages="raErrors" :counter="140" label="RA"
              placeholder="Informe o registro acadêmico" required @input="$v.ra.$touch()" @blur="$v.ra.$touch()" />

            <v-text-field disabled outlined v-model="cpf" :error-messages="cpfErrors" :counter="14" label="CPF"
              placeholder="Informe o número do documento" required @input="$v.cpf.$touch()" @blur="$v.cpf.$touch()" />
          </v-card-text>

          <v-card-actions>
            <v-btn type="submit" class="primary text-capitalize mr-4" elevation="0">
              Salvar
            </v-btn>
            <v-btn class="text-capitalize" elevation="0" to="/">
              Cancelar
            </v-btn>
          </v-card-actions>
        </form>
      </v-card>

      <v-alert :value="showAlert" :type="alertType" transition="scale-transition" class="mt-8">
        {{ alertText }}
      </v-alert>
    </v-col>
  </v-row>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, minLength, maxLength, email } from 'vuelidate/lib/validators'

export default {
  name: 'NewStudent',

  mixins: [validationMixin],

  validations: {
    name: { required, maxLength: maxLength(80) },
    email: { required, email, maxLength: maxLength(80) },
    ra: { required, maxLength: maxLength(140) },
    cpf: { required, minLength: minLength(11), maxLength: maxLength(14) },
  },

  async asyncData({ $axios, params }) {
    const { name, email, ra, cpf } = await $axios.$get(`/students/${params.id}`);

    return { name, email, ra, cpf }
  },

  data() {
    return {
      showAlert: false,
      alertText: '',
      alertType: 'success',
    }
  },

  computed: {
    nameErrors() {
      const errors = []
      if (!this.$v.name.$dirty) return errors
      !this.$v.name.required && errors.push('Nome é obrigatório')
      !this.$v.name.maxLength && errors.push('Nome deve ter no máximo 80 caracteres')
      return errors
    },
    emailErrors() {
      const errors = []
      if (!this.$v.email.$dirty) return errors
      !this.$v.email.required && errors.push('E-mail é obrigatório')
      !this.$v.email.email && errors.push('Deve informar um e-mail válido')
      !this.$v.email.maxLength && errors.push('Nome deve ter no máximo 80 caracteres')
      return errors
    },
    raErrors() {
      const errors = []
      if (!this.$v.ra.$dirty) return errors
      !this.$v.ra.required && errors.push('Registro acadêmico é obrigatório')
      !this.$v.ra.maxLength && errors.push('Nome deve ter no máximo 140 caracteres')
      return errors
    },
    cpfErrors() {
      const errors = []
      if (!this.$v.cpf.$dirty) return errors
      !this.$v.cpf.required && errors.push('CPF é obrigatório')
      !this.$v.cpf.minLength && errors.push('Nome deve ter no mínimo 11 caracteres')
      !this.$v.cpf.maxLength && errors.push('Nome deve ter no máximo 14 caracteres')
      return errors
    },
  },

  methods: {
    async submit() {
      try {
        await this.$axios.$patch(`/students/${this.$route.params.id}`, {
          name: this.name,
          email: this.email,
        });

        this.name = '';
        this.email = '';

        this.showAlert = true;
        this.alertText = 'Aluno atualizado com sucesso!';
        this.alertType = 'success';
      } catch (error) {
        this.showAlert = true;
        this.alertText = 'Erro ao atualizar o aluno!';
        this.alertType = 'error';
      }
    },
  },
}
</script>
