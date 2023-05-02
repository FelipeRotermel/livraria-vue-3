<script>
import LivrosApi from "@/api/livros";
import AutoresApi from "@/api/autores";
import EditorasApi from "@/api/editoras";
import CategoriasApi from "@/api/categorias";
const livrosApi = new LivrosApi();
const autoresApi = new AutoresApi();
const editorasApi = new EditorasApi();
const categoriasApi = new CategoriasApi();
export default {
  data() {
    return {
      categorias: [],
      editoras: [],
      autores: [],
      livros: [],
      livro: {},
      editora: {},
      categoria: {},
    };
  },
  async created() {
    this.livros = await livrosApi.buscarTodosOsLivros();
    this.autores = await autoresApi.buscarTodosOsAutores();
    this.editoras = await editorasApi.buscarTodasAsEditoras();
    this.categorias = await categoriasApi.buscarTodasAsCategorias();
  },
  methods: {
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
  <div class="container-fluid">
    <h1 class="titulo">Livros</h1>
    <div class="infoContent">
      <hr />
      <div class="form input-group">
        <input class="form-control" type="text" v-model="livro.titulo" placeholder="Título" />
        <input class="form-control" type="text" v-model="livro.isbn" placeholder="ISBN" />
        <input class="form-control" type="text" v-model="livro.quantidade" placeholder="Quantidade" />
        <input class="form-control" type="text" v-model="livro.preco" placeholder="Preço" />

        <label>Autor:</label>
        <select class="form-select" v-model="livro.autor">
          <option v-for="autor in autores" :value="autor.id">{{ autor.nome }}</option>
        </select>

        <label>Editora:</label>
        <select class="form-select" v-model="livro.editora">
          <option v-for="editora in editoras" :value="editora.id">{{ editora.nome }}</option>
        </select>

        <label>Categoria:</label>
        <select class="form-select" v-model="livro.categoria">
          <option v-for="categoria in categorias" :value="categoria.id">{{ categoria.descricao }}</option>
        </select>

        <button class="btn btn-outline-secondary" @click="salvar">Salvar</button>
      </div>
      <hr />
    </div>
    <ul>
      <li v-for="livro in livros" :key="livro.id">
        <span class="spanLivro" @click="editar(livro)">
          ({{ livro.id }}) - {{ livro.titulo }} - {{ livro.isbn }} - {{ livro.quantidade }} - {{ livro.preco }}
        </span>
        <button type="button" class="btn btn-danger" @click="excluir(livro)">X</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>

.spanLivro {
  cursor: pointer;
  font-size: large;
  margin-right: 10px;
}

.input-group {
  display: flex;
  text-align: center;
  justify-content: center;
  align-items: center;
  margin: 0;
  padding: 0;
}
</style>
