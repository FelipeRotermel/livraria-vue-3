<script>
  import CategoriasApi from "../api/categorias.js";
  const categoriasApi = new CategoriasApi();
  export default {
    data() {
      return {
        categorias: [],
        categoria: {},
        indice_editar: -1,
      };
    },
    async created() {
      this.categorias = await categoriasApi.buscarTodasAsCategorias();
    },
    methods: {
      async salvar() {
        if (this.categoria.id) {
          await categoriasApi.atualizarCategoria(this.categoria);
        } else {
          await categoriasApi.adicionarCategoria(this.categoria);
        }
        this.categorias = await categoriasApi.buscarTodasAsCategorias();
        this.categoria = {};
      },
      async excluir(categoria) {
        await categoriasApi.excluirCategoria(categoria.id);
        this.categorias = await categoriasApi.buscarTodasAsCategorias();
      },
      editar(categoria) {
        Object.assign(this.categoria, categoria);
      },
    },
  };
</script>
<template>
  <div class="tudo">
    <div class="title">
      <h2>Gerenciamento de categorias</h2>
    </div>
    <div class="form input-group">
      <input
        @keyup.enter="salvar"
        v-model="categoria.descricao"
        type="text"
        placeholder="Descrição da categoria"
        class="form-control"
      />

      <button class="btn btn-outline-secondary" @click="salvar">Salvar</button>
    </div>
    <div class="list-categorias">
      <table>
        <thead>
          <tr>
            <th>ID-categoria</th>
            <th>Descrição</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="categoria in categorias" :key="categoria.id">
            <td>
              {{ categoria.id }}
            </td>
            <td>
              {{ categoria.descricao }}
            </td>
            <td>
              <button class="btn btn-outline-secondary" @click="editar(categoria)">Editar</button>
              <button class="btn btn-outline-secondary" @click="excluir(categoria)">Excluir</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>