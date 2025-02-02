<template>

  <nav class="navbar navbar-expand-lg bg-success">
    <div class="container-fluid">
      <a class="navbar-brand fc" href="/">VUE.js</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup"
        aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <router-link to="/" class="nav-link fc">Home</router-link>
          <router-link to="/about" class="nav-link fc">About</router-link>
          <router-link to="/empleados" class="nav-link fc">Empleados</router-link>
        </div>
      </div>
    </div>
  </nav>
  <div class="container">
    <router-view />
  </div>

</template>

<style lang="scss">
.fc {
  color: white;
}

.fc:hover {
  color: aquamarine;
}
</style>
