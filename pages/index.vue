<template>
  <v-row justify="center" align="center">
    <v-col cols="12">
      <v-row class="mt-8">
        <v-col cols="12">
          <v-btn color="primary text-capitalize" elevation="0" large to="/new_student">
            Cadastrar Aluno
          </v-btn>
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
                    <v-icon small @click="deleteItem">
                      mdi-delete
                    </v-icon>
                  </td>
                </tr>

                <v-dialog v-model="dialogDelete" max-width="500px">
                  <v-card>
                    <v-card-title class="text-h5">
                      Tem certeza que deseja excluir o aluno?
                    </v-card-title>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn class="red white--text" elevation="0" @click="deleteItemConfirm(row.item.id)">Excluir
                      </v-btn>
                      <v-btn elevation="0" @click="closeDelete">Cancelar</v-btn>
                      <v-spacer></v-spacer>
                    </v-card-actions>
                  </v-card>
                </v-dialog>
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
      dialogDelete: false,
    }
  },

  methods: {
    editItem(item) {
      console.log(item);
      // this.editedIndex = this.students.indexOf(item)
      // this.editedItem = Object.assign({}, item)
      // this.dialog = true
    },

    deleteItem() {
      this.dialogDelete = true;
    },

    async deleteItemConfirm(id) {
      try {
        await this.$axios.$delete(`/students/${id}`);

        this.students = await this.$axios.$get('/students');

        this.dialogDelete = false;
      } catch (error) {
        console.log('Error on delete student');
      }
    },

    closeDelete() {
      this.dialogDelete = false;
    },
  },
}
</script>
