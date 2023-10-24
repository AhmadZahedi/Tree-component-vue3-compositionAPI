<template>
  <header>
    Hello
  </header>

  <main>
    <TreeComponent />
  </main>
</template>

<script>
import TreeComponent from "./components/TreeComponent.vue";

export default {
  components: {
    TreeComponent
  },
  setup() {
    return {}
  }
}
</script>
