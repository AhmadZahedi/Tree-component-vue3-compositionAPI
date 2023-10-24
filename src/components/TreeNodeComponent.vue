<template>
  <li>
    <div class="d-flex">
      <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="none" viewBox="0 0 24 24">
        <path fill="#00e1ff" d="M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" opacity=".1"/>
        <path stroke="#00e1ff" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/>
        <path stroke="#00e1ff" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="m11 15 2.716-2.716v0a.402.402 0 0 0 0-.568v0L11 9"/>
      </svg>

      <input
          type="checkbox"
          :id="node[itemValue]"
          :checked="node.__selected"
      >

      <label :for="node[itemValue]">
        {{ node[itemTitle] }}
      </label>
    </div>
  </li>

  <ul v-if="node.children">
    <TreeNodeComponent
        v-for="child in node.children"
        :node="child"
        :item-title="itemTitle"
        :item-value="itemValue"
    />
  </ul>
</template>

<script>
export default {
  props: {
    node: {
      type: Object,
      required: true
    },
    itemValue: {
      type: String,
      required: true
    },
    itemTitle: {
      type: String,
      required: true
    }
  },
  setup() {
    return {}
  }
}
</script>

<style scoped>
li {
  list-style: none;
}
</style>
