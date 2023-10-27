<template>
  Tree component
  <ul>
    <TreeNodeComponent
        v-for="node in data"
        :node="node"
        :itemValue="itemValue"
        :itemTitle="itemTitle"
    />
  </ul>
</template>

<script>
import TreeNodeComponent from "./TreeNodeComponent.vue";
import {ref} from "vue";

export default {
  components: {TreeNodeComponent},

  props: {
    rawData: {
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
    console.log(props.rawData)

    function prepareData(array, parentId = null) {
      let result = [];
      array.forEach((item) => {
        item.__selected = false;
        item.__expanded = true;
        if (item.parent_id === parentId) {
          result.push(item);
          item.children = prepareData(array, item.id);
        }
      })
      return result;
    }

    const data = ref(prepareData(props.rawData));

    return {
      data
    };
  }
}
</script>
