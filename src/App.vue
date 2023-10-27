<template>
  <header>
    Hello
  </header>

  <main>
    <div class="vw-100 d-flex justify-content-center">
      <TreeComponent
          :rawData="rawData"
          item-title="label"
          item-value="id"
      />
    </div>
  </main>
</template>

<script>
import TreeComponent from "./components/TreeComponent.vue";

export default {
  components: {
    TreeComponent
  },
  setup() {
    const rawData = [];

    for (let i = 1; i <= 100; i++) {
      const parent_id = i > 1 ? Math.floor(Math.random() * (i - 1)) + 1 : null;
      const label = `Item ${i}`;
      const value = `item_${i}`;

      rawData.push({id: i, parent_id, label, value});
    }



    return {
      rawData
    };
  }
}
</script>
