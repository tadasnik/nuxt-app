<template>
<nav class="navbar is-info" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="https://bulma.io">
      <img src="https://bulma.io/images/bulma-logo.png" width="112" height="28">
    </a>

    <a role="button" @click="makeBurger" class="navbar-burger burger" aria-label="menu" aria-expanded="false" data-target="navMenu" v-bind:class="{ 'is-active': activator }">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>

  <div id="navMenu" class="navbar-menu" v-bind:class="{ 'is-active': activator }">
    <div class="navbar-start">
      <nuxt-link to="/" class="navbar-item" active-class="is-active" exact >Home</nuxt-link>


          <nuxt-link to="/research" class="navbar-item" active-class="is-active" exact>Research</nuxt-link>

          <nuxt-link to="/maps" class="navbar-item" active-class="is-active" exact>Fire maps</nuxt-link>

          <nuxt-link to="/zooming" class="navbar-item" active-class="is-active" exact>zooming</nuxt-link>
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
  data () {
    return {
      msg: '',
      activator: false
    }
  },
  methods: {
    makeBurger () {
      this.activator = !this.activator
      return this.activator
    }
  }
}
</script>


