<script>
  import AutoresApi from "../api/autores";
  const autoresApi = new AutoresApi();
  export default {
    data() {
      return {
        autores: [],
        autor: {},
        indice_editar: -1,
      };
    },
    async created() {
      this.autores = await autoresApi.buscarTodosOsAutores();
    },
    methods: {
      async salvar() {
        if (this.autor.id) {
          await autoresApi.atualizarAutor(this.autor);
        } else {
          await autoresApi.adicionarAutor(this.autor);
        }
        this.autores = await autoresApi.buscarTodosOsAutores();
        this.autor = {};
      },
      async excluir(autor) {
        await autoresApi.excluirAutor(autor.id);
        this.autores = await autoresApi.buscarTodosOsAutores();
      },
      editar(autor) {
        Object.assign(this.autor, autor);
      },
    },
  };
</script>
<template>
  <div class="tudo">
    <div class="title">
      <h2>Gerenciamento de autores</h2>
    </div>
    <div class="form input-group">
      <input
        @keyup.enter="salvar"
        v-model="autor.nome"
        type="text"
        placeholder="Nome do autor"
        class="form-control"
      />
      <input
        @keyup.enter="salvar"
        v-model="autor.email"
        type="text"
        placeholder="Email"
        class="form-control"
      />

      <button class="btn btn-outline-secondary" @click="salvar">Salvar</button>
    </div>
    <div class="list-autores">
      <table>
        <thead>
          <tr>
            <th>ID-autor</th>
            <th>Nome</th>
            <th>Email</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="autor in autores" :key="autor.id">
            <td>
              {{ autor.id }}
            </td>
            <td>
              {{ autor.nome }}
            </td>
            <td>
              {{ autor.email }}
            </td>
            <td>
              <button class="btn btn-outline-secondary" @click="editar(autor)">Editar</button>
              <button class="btn btn-outline-secondary" @click="excluir(autor)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>