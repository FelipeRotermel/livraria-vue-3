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
    livros: [],
    livro: {
      autor_id: null,
      editora_id: null,
      categoria_id: null,
    },
    autores: [],
    editoras: [],
    categorias: [],
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
    const autor = this.autores.find(a => a.id === this.livro.autor_id);
    const editora = this.editoras.find(e => e.id === this.livro.editora_id);
    const categoria = this.categorias.find(i => i.id === this.livro.categoria_id);

    await livrosApi.adicionarLivro({ ...this.livro, autor, editora, categoria });
  }
  this.livro = { autor_id: null };
  this.livro = { editora_id: null };
  this.livro = { categoria_id: null };
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

  <label for="editora-select">Editora:</label>
  <select id="editora-select" v-model="livro.editora_id">
    <option v-for="editora in editoras" :value="editora.id">{{ editora.nome }}</option>
  </select>

  <label for="categoria-select">Categoria:</label>
  <select id="categoria-select" v-model="livro.categoria_id">
    <option v-for="categoria in categorias" :value="categoria.id">{{ categoria.descricao }}</option>
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
