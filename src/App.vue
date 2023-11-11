<template>
  <header>
    Hello
  </header>

  <main>
    <div class="vw-100 d-flex justify-content-center">
      <TreeComponent
          v-model="result"
          :raw-data="data"
          item-title="label"
          item-value="id"
          :dependent="false"
          @update:modelValue="finish"
      />
    </div>
  </main>
</template>

<script>
import TreeComponent from "./components/TreeComponent.vue";
import {ref} from "vue";

export default {
  components: {
    TreeComponent
  },
  setup() {
    const data = [];

    for (let i = 1; i <= 100; i++) {
      const parent_id = i > 1 ? Math.floor(Math.random() * (i - 1)) + 1 : null;
      const label = `Item ${i}`;
      const value = `item_${i}`;

      data.push({id: i, parent_id, label, value});
    }

    const result = ref([1, 3, 2, 8]);

    function finish(outputArr) {
      console.log('finished ', outputArr);
    }

    return {
      data,
      result,
      finish
    };
  }
}
</script>
