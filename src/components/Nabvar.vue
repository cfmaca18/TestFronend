<template>
  <nav class="navbar navbar-expand-md navbar-light">
    <div class="container-fluid">
      <div class="navbar-brand">
        <router-link to="/">Inicio</router-link>
      </div>

      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbar-collapse"
        aria-controls="navbar-collapse"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbar-collapse">
        <div class="navbar-nav me-auto mb-2 mb-lg-0">
          <router-link class="nav-link" to="/ver-proyecto">Proyectos</router-link>
          <router-link class="nav-link" to="/crear-grupo">Crear Grupo</router-link>
        </div>

        <div class="navbar-nav ms-auto mb-2 mb-lg-0">
          <template v-if="$store.state.user.isAuthenticated">
            <router-link class="nav-link" to="/crear-proyecto">
              Crear proyecto
            </router-link>
            <button class="nav-link" @click="logout">Logout</button>
          </template>

          <template v-else>
            <router-link class="nav-link" to="/login">Login</router-link>
            <router-link class="nav-link" to="/registrar">Registrar</router-link>
          </template>
        </div>
      </div>
    </div>
  </nav>
</template>

<script>
import axios from "axios";

export default {

  methods: {
    async logout() {
      try {
        const config = {
          headers: { Authorization: `Bearer ${localStorage.getItem("access_token")}` },
      };
      await axios.post("/api/token/logout/", null, config);
      localStorage.removeItem("access_token");
      this.$store.commit("user/removeToken");
      this.$router.push("/");
    } catch (error) {
      console.log(error);
    }
  },

  },
};
</script>

<style>
.navbar {
  padding: 1rem;
  background-color: #16dfce;
}

.navbar-brand {
  font-size: 1.5rem;
  font-weight: bold;
}

.nav-link {
  color: #fff;
  margin: 0 1rem;
  font-size: 1.2rem;
}

.nav-link:hover {
  color: #fff;
  text-decoration: underline;
}
</style>
