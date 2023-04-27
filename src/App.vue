<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
    <div class="container-fluid">
      <div class="collapse navbar-collapse" id="navbarScroll">
        <ul
          class="navbar-nav me-auto my-2 my-lg-0 navbar-nav-scroll"
          style="--bs-scroll-height: 100px"
        >
          <li class="nav-item">
            <div class="nav-link active"><router-link :to="{ name: 'home' }">Home</router-link> </div>  
          </li>
          <li class="nav-item">
            <div class="nav-link active"><router-link to="/categorias">Categorias</router-link> </div>  
          </li>
          <li class="nav-item">
            <div class="nav-link active"><router-link to="/autores">Autores</router-link> </div>  
          </li>
          <li class="nav-item">
            <div class="nav-link active"><router-link to="/editoras">Editoras</router-link> </div>  
          </li>
          <li class="nav-item">
            <div class="nav-link active"><router-link to="/livros">Livros</router-link> </div>  
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <main>
    <RouterView />
  </main>
</template>
<style>
.navbar {
  font-size: 32px;
}
.form-control {
  margin: 2px;
}
.titulo {
  padding-top: 80px;
}
</style>
