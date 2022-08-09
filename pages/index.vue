<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-row class="mt-8">
        <v-col cols="12">
          <v-btn color="primary" elevation="0" large>Cadastrar Aluno</v-btn>
        </v-col>
      </v-row>

      <v-row class="mt-8">
        <v-col cols="12">
          <v-card>
            <v-card-title>
              Alunos
              <v-spacer></v-spacer>
              <v-text-field v-model="search" append-icon="mdi-magnify" label="Digite sua busca" single-line
                hide-details>
              </v-text-field>
            </v-card-title>
            <v-data-table :headers="headers" :items="students" :search="search">
              <template v-slot:item="row">
                <tr>
                  <td>{{ row.item.ra }}</td>
                  <td>{{ row.item.name }}</td>
                  <td>{{ row.item.cpf }}</td>
                  <td>
                    <v-icon small class="mr-2" @click="editItem(row)">
                      mdi-pencil
                    </v-icon>
                    <v-icon small @click="deleteItem(row)">
                      mdi-delete
                    </v-icon>
                  </td>
                </tr>
              </template>
            </v-data-table>
          </v-card>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
export default {
  name: 'IndexPage',

  async asyncData({ $axios }) {
    const students = await $axios.$get('/students');

    return { students }
  },

  data() {
    return {
      search: '',
      headers: [
        { text: 'Registro Acadêmico', value: 'ra' },
        { text: 'Nome', value: 'name' },
        { text: 'CPF', value: 'cpf' },
        { text: 'Ações', value: 'actions', sortable: false },
      ],
    }
  },

  methods: {
    editItem(item) {
      console.log(item);
      // this.editedIndex = this.students.indexOf(item)
      // this.editedItem = Object.assign({}, item)
      // this.dialog = true
    },

    deleteItem(item) {
      console.log(item);
      // this.editedIndex = this.students.indexOf(item)
      // this.editedItem = Object.assign({}, item)
      // this.dialogDelete = true
    },
  },
}
</script>
