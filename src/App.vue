<template>
  <div id="app">
    <router-view></router-view>
    <footer class="flex items-center text-center">
      <div
        class="flex flex-1 flex-col justify-between items-center w-full"
      ></div>
    </footer>
  </div>
</template>

<script>
export default {
  name: "app",

  data() {
    return {
      pageTitle: "Quickly find out which HTML tag to use when",
    };
  },

  computed: {
    breadcrumbs() {
      const paths = this.$route.path.split("/");

      return paths.filter((part) => part !== "");
    },
  },
};
</script>

<style>
#app {
  display: grid;
  width: 100%;
  min-height: 100vh;
  grid-template-rows: 1fr minmax(8rem, 15vh);
  grid-row-gap: 0;
}

main {
  padding: 1rem 0;
}

footer .breadcrumbs li:not(:last-child)::after {
  content: "/";
  padding: 0 1rem;
}

.cred li {
  display: inline;
}

.cred li:not(:last-child)::after {
  content: ", ";
}
</style>
