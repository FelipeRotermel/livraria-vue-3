<script>
import LivrosApi from "@/api/livros";
import AutoresApi from "@/api/autores";
const livrosApi = new LivrosApi();
const autoresApi = new AutoresApi();
export default {
  data() {
    return {
      livros: [],
      livro: {},
      autores: [],
      autor: {},
    };
  },
  async created() {
    this.livros = await livrosApi.buscarTodosOsLivros();
    this.autores = await autoresApi.buscarTodosOsAutores();
  },
  methods: {
    async salvar() {
      if (this.livro.id) {
        await livrosApi.atualizarLivro(this.livro);
        await autoresApi.atualizarAutor(this.autor);
      } else {
        await livrosApi.adicionarLivro(this.livro);
        await autoresApi.adicionarAutor(this.autor);
      }
      this.livro = {};
        this.autor = {};
      this.livros = await livrosApi.buscarTodosOsLivros();
        this.autores = await autoresApi.buscarTodosOsAutores();
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
  <h1>Livros</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="livro.titulo" placeholder="Título" />
    <input type="text" v-model="livro.isbn" placeholder="ISBN" />
    <input type="text" v-model="livro.quantidade" placeholder="Quantidade" />
    <input type="text" v-model="livro.preco" placeholder="Preço" />
    <input type="text" v-model="livro.categoria" placeholder="Categoria" />
    <input type="text" v-model="livro.editora" placeholder="Editora" />
    <input type="text" v-model="livro.autor" placeholder="Autor" />
    <button @click="salvar">Salvar</button>
  </div>
  <hr />
  <ul>
    <li v-for="livro in livros" :key="livro.id">
      <span @click="editar(livro)">
        ({{ livro.id }}) - {{ livro.titul }} -
      </span>
      <button @click="excluir(livro)">X</button>
    </li>
  </ul>
</template>

<style></style>
