<template>
Tree component
<TreeNode
    v-for="node in data"
    :node="node"
    :itemValue="itemValue"
    :itemTitle="itemTitle"
/>
</template>

<script>
import TreeNode from "./TreeNodeComponent.vue";
export default {
  components: { TreeNode },
  props: {
    data: {
      type: Array,
      required: true
    },
    itemValue: {
      type: String,
      default: 'value'
    },
    itemTitle: {
      type: String,
      default: 'title'
    }
  },
  setup(props) {
    console.log(props.data)

    return {}
  }
}
</script>
