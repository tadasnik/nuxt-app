<template>
  <nav class="navbar is-info" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
      <a class="navbar-item" href="https://bulma.io">
        <img src="https://bulma.io/images/bulma-logo.png" width="112" height="28">
      </a>

      <a
        role="button"
        class="navbar-burger burger"
        aria-label="menu"
        aria-expanded="false"
        data-target="navMenu"
        :class="{ 'is-active': activator }"
        @click="makeBurger"
      >
        <span aria-hidden="true" />
        <span aria-hidden="true" />
        <span aria-hidden="true" />
      </a>
    </div>

    <div id="navMenu" class="navbar-menu" :class="{ 'is-active': activator }">
      <div class="navbar-start">
        <nuxt-link to="/" class="navbar-item" active-class="is-active" exact>
          Home
        </nuxt-link>


        <nuxt-link to="/research" class="navbar-item" active-class="is-active" exact>
          Research
        </nuxt-link>

        <nuxt-link to="/maps" class="navbar-item" active-class="is-active" exact>
          Fire maps
        </nuxt-link>

        <nuxt-link to="/maps_panzoom" class="navbar-item" active-class="is-active" exact>
          maps_panzoom
        </nuxt-link>


        <nuxt-link to="/zooming" class="navbar-item" active-class="is-active" exact>
          zooming
        </nuxt-link>

        <nuxt-link to="/zoomap" class="navbar-item" active-class="is-active" exact>
          zooming_mod
        </nuxt-link>

        <div class="navbar-item has-dropdown is-hoverable">
          <a class="navbar-link">
            More
          </a>

          <div class="navbar-dropdown">
            <a class="navbar-item">
              About
            </a>
            <a class="navbar-item">
              Jobs
            </a>
            <a class="navbar-item">
              Contact
            </a>
            <hr class="navbar-divider">
            <a class="navbar-item">
              Report an issue
            </a>
          </div>
        </div>
      </div>

      <div class="navbar-end">
        <div class="navbar-item">
          <div class="buttons">
            <a class="button is-primary">
              <strong>Sign up</strong>
            </a>
            <a class="button is-light">
              Log in
            </a>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>
<script>
export default {
  name: 'NavbarApp',
  data() {
    return {
      msg: '',
      activator: false
    }
  },
  methods: {
    makeBurger() {
      this.activator = !this.activator
      return this.activator
    }
  }
}
</script>
