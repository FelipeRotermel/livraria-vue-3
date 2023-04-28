<script>
import LivrosApi from "@/api/livros";
import EditorasApi from "@/api/editoras";
import AutoresApi from "@/api/autores";
import CatagoriasApi from "@/api/categorias";
const livrosApi = new LivrosApi();
const editorasApi = new EditorasApi();
const autoresApi = new AutoresApi();
const categoriasApi = new CatagoriasApi();
export default {
  data() {
  return {
    livros : [],
    livro: {},
    autor: {},
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
    await livrosApi.atualizarLivro(this.livro);
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
  <h1 class="titulo">Livros</h1>
  <hr />
  <div class="form input-group">
  <input class="form-control" type="text" v-model="livro.titulo" placeholder="Título" />
  <input class="form-control" type="text" v-model="livro.isbn" placeholder="ISBN" />
  <input class="form-control" type="text" v-model="livro.quantidade" placeholder="Quantidade" />
  <input class="form-control" type="text" v-model="livro.preco" placeholder="Preço" />

  <label for="autor-select">Autor:</label>
  <select id="autor-select" v-model="livro.autor">
    <option v-for="autor in autores" :value="autor.id">{{ autor.nome }}</option>
  </select>

  <label for="editora-select">Editora:</label>
  <select id="editora-select" v-model="livro.editora">
    <option v-for="editora in editoras" :value="editora.id">{{ editora.nome }}</option>
  </select>

  <label for="categoria-select">Categoria:</label>
  <select id="categoria-select" v-model="livro.categoria">
    <option v-for="categoria in categorias" :value="categoria.id">{{ categoria.descricao }}</option>
  </select>

  <button class="btn btn-outline-secondary" @click="salvar">Salvar</button>
</div>
  <hr />
  <ul>
    <li v-for="livro in livros" :key="livro.id">
      <span @click="editar(livro)">
        ({{ livro.id }}) - {{ livro.titulo }} - {{ livro.isbn }} - {{ livro.quantidade }} - {{ livro.preco }}
      </span>
      <button @click="excluir(livro)">X</button>
    </li>
  </ul>
</template>

