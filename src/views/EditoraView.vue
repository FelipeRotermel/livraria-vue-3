<script>
  import EditorasApi from "../api/editoras";
  const editorasApi = new EditorasApi();
  export default {
    data() {
      return {
        editoras: [],
        editora: {},
        indice_editar: -1,
      };
    },
    async created() {
      this.editoras = await editorasApi.buscarTodasAsEditoras();
    },
    methods: {
      async salvar() {
        if (this.editora.id) {
          await editorasApi.atualizarEditora(this.editora);
        } else {
          await editorasApi.adicionarEditora(this.editora);
        }
        this.editoras = await editorasApi.buscarTodasAsEditoras();
        this.editora = {};
      },
      async excluir(editora) {
        await editorasApi.excluirEditora(editora.id);
        this.editoras = await editorasApi.buscarTodasAsEditoras();
      },
      editar(editora) {
        Object.assign(this.editora, editora);
      },
    },
  };
</script>

<template>
  <div class="tudo">
    <div class="title">
      <h2>Gerenciamento de Editoras</h2>
    </div>
    <div class="form input-group">
      <input
        @keyup.enter="salvar"
        v-model="editora.nome"
        type="text"
        placeholder="Nome do editora"
        class="form-control"
      />
      <input
        @keyup.enter="salvar"
        v-model="editora.site"
        type="text"
        placeholder="Nome do site"
        class="form-control"
      />
      <button class="btn btn-outline-secondary" @click="salvar">Adicionar</button>
    </div>
    <div class="list-editoras">
      <table>
        <thead>
          <tr>
            <th>ID-Editora</th>
            <th>Nome</th>
            <th>Site</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="editora in editoras" :key="editora.id">
            <td>
              {{ editora.id }}
            </td>
            <td>
              {{ editora.nome }}
            </td>
            <td>
              <a target="blank" :href="editora.site"> {{ editora.site }}</a>
            </td>
            <td>
              <button class="btn btn-outline-secondary" @click="editar(editora)">Editar</button>
              <button class="btn btn-outline-secondary" @click="excluir(editora)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>