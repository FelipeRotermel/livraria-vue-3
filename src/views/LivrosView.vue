<script>
import LivrosApi from "@/api/livros";
const livrosApi = new LivrosApi();
import AutoresApi from "@/api/autores";
const autoresApi = new AutoresApi();
import EditorasApi from "@/api/editoras";
const editorasApi = new EditorasApi();
import CategoriasApi from "@/api/categorias";
const categoriasApi = new CategoriasApi();
export default {
  data() {
    return {
      autores: [],
      editora: {},
      editoras: [],
      categoria: {},
      categorias: [],
      itemSelecionado: null,
    };
  },
  async created() {
    this.livros = await livrosApi.buscarTodosOsLivros();
    this.autores = await autoresApi.buscarTodosOsAutores();
    this.editoras = await editorasApi.buscarTodasAsEditoras();
    this.categorias = await categoriasApi.buscarTodasAsCategorias();
  },
  methods: {
    async selecionarItem(livro) {
      this.itemSelecionado = livro;
    },

    async salvar() {
      if (this.livro.id) {
        await livrosApi.atualizarLivro(this.livro);
      } else {
        await livrosApi.adicionarLivro(this.livro);
      }
      this.livro = {};
      this.livros = await livrosApi.buscarTodosOsLivros();
    },
    editar(livro) {
      Object.assign(this.livro, livro);
    },
    async excluir(livro) {
      await livrosApi.excluirLivro(livro.id);
      this.livros = await livrosApi.buscarTodosOsLivros();
    },
  },
};
</script>

<template>
  <div class="tudo">
    <div class="title">
      <h2>Gerenciamento de Livros</h2>
    </div>
    <div class="form input-group">
      <input
        class="form-control"
        type="text"
        v-model="livro.titulo"
        placeholder="Nome"
      />
      <input
        class="form-control"
        type="text"
        v-model="livro.isbn"
        placeholder="ISBN"
      />
      <input
        class="form-control"
        type="text"
        v-model="livro.quantidade"
        placeholder="Quantidade"
      />
      <input
        class="form-control"
        type="text"
        v-model="livro.preco"
        placeholder="Preço"
      />
      </div>
      <div class="form input-group">
      <select class="" v-model="livro.autor" id="">
        <option v-for="autor in autores" :key="autor.id" :value="autor.id">
          {{ autor.nome }}
        </option>
      </select>
      <select class="" v-model="livro.editora" id="">
        <option
          v-for="editora in editoras"
          :key="editora.id"
          :value="editora.id"
        >
          {{ editora.nome }}
        </option>
      </select>
      <select class="" v-model="livro.categoria" id="">
        <option
          v-for="categoria in categorias"
          :key="categoria.id"
          :value="categoria.id"
        >
          {{ categoria.descricao }}
        </option>
      </select>
      <button class="btn btn-outline-secondary" @click="salvar">Salvar</button>
    </div>
    <hr />
    <table>
      <thead>
        <tr >
          <th>ID</th>
          <th>Nome</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody>
        <tr class="listaCategoria" v-for="livro in livros" :key="livro.id">
          <td scope="row" @click="selecionarItem(livro)">
            <div>{{ livro.id }}</div>
          </td>
          <td>
            <div>{{ livro.titulo }}</div>
          </td>
          <td>
            <button class="btn btn-outline-secondary" @click="editar(editora)">Editar</button>
            <button class="btn btn-outline-secondary" @click="excluir(editora)">Excluir</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style>
.form-control {
  margin: 2px;
}
.titulo {
  padding-top: 80px;
}
.tudo {
  padding-top: 80px;
}
</style>