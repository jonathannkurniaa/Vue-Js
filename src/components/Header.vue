<template>
  <header>
    <nav class="navbar navbar-expand-lg bg-light" >
      <div style="padding-left:40px" class="container-fluid">
        
        <router-link class="navbar-brand"  s to="/">Kulkas Online </router-link>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div  style="margin-left:400px" class="collapse navbar-collapse" id="navbarNav">
          <ul   class="navbar-nav">
            <li  class="nav-item">
              <!-- <a class="nav-link active" aria-current="page" href="#">Isi Kulkas</a> -->
              <router-link  class="nav-link active"  aria-current="page" to="/"
                >Home</router-link
              >
            </li>
            <li style="margin-left:50px " class="nav-item">
              <router-link class="nav-link" to="/keranjang"
                >Keranjang</router-link
              >
            </li>
            <!-- <router-link to="/">Home</router-link> |
      <router-link to="/keranjang">Keranjang</router-link> -->
          </ul>
        </div>
      </div>
    </nav>
  </header>
</template>

<style></style>
