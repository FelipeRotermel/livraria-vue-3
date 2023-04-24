<script>
import LivrosApi from "@/api/livros";
const livrosApi = new LivrosApi();
import AutoresApi from "@/api/autores";
const autoresApi = new AutoresApi();
export default {
  data() {
  return {
    livros: [],
    livro: {
      autor_id: null,
    },
    autores: [],
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
  } else {
    await livrosApi.adicionarLivro({ ...this.livro, autor: `/autores/${this.livro.autor_id}/` });
  }
  this.livro = { autor_id: null };
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
  <h1>Livros</h1>
  <hr />
  <div class="form">
  <input type="text" v-model="livro.titulo" placeholder="Título" />
  <input type="text" v-model="livro.isbn" placeholder="ISBN" />
  <input type="text" v-model="livro.quantidade" placeholder="Quantidade" />
  <input type="text" v-model="livro.preco" placeholder="Preço" />

  <label for="autor-select">Autor:</label>
  <select id="autor-select" v-model="livro.autor_id">
    <option v-for="autor in autores" :value="autor.id">{{ autor.nome }}</option>
  </select>

  <button @click="salvar">Salvar</button>
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

<style></style>
