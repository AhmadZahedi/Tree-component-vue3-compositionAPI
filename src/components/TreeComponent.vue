<template>
  Tree component
  <ul>
    <TreeNodeComponent
        v-for="node in tree"
        :key="node[itemValue]"
        :node="node"
        :itemTitle="itemTitle"
        :itemValue="itemValue"
        :dependent="dependent"
        @addNodeIdToResult="updateOutput"
        @addAllNodesIdToResult="selectAll(rawData)"
        @emptyOutput="deleteAll(rawData)"
    />
  </ul>
</template>

<script>
import TreeNodeComponent from "./TreeNodeComponent.vue";
import {computed, onMounted, ref} from "vue";

export default {
  components: {TreeNodeComponent},

  props: {
    modelValue: {
      type: Array,
      required: true
    },
    rawData: {
      type: Array,
      required: true
    },
    itemTitle: {
      type: String,
      default: 'title'
    },
    itemValue: {
      type: String,
      default: 'value'
    },
    dependent: {
      type: Boolean,
      default: true
    }
  },

  emits: ['update:modelValue'],

  setup(props , { emit }) {
    let output = [];

    const tree = computed(() => {
      return buildTree(props.rawData);
    });

    function buildTree(data, parentId = null) {
      const temp = [];

      data.forEach(item => {
        item.__selected = false;
        item.__expanded = false;
        if (item.parent_id === parentId) {
          temp.push(item);
          const children = buildTree(data, item.id);

          if (children.length > 0) item.children = children;
        }
      });

      return temp;
    }

    function addSelectedItems(arr) {
      arr.forEach(item => {
        if (props.modelValue.includes(item[props.itemValue])) {
          item.__selected = true;
        }
      });
    }

    function updateModelValue() {
      emit('update:modelValue', output);
    }

    function updateOutput(item) {
      const stack = [item];

      while (stack.length > 0) {
        const currentItem = stack.pop();

        const needToCheckChildren = props.dependent && currentItem.children;

        if (!currentItem.__selected) {
          // Item not selected, add it to the output
          currentItem.__selected = true;
          output.push(currentItem[props.itemValue]);

          if (needToCheckChildren) {
            // Add children to the stack
            currentItem.children.forEach((child) => {
              if (!child.__selected) {
                stack.push(child);
              }
            });
          }
        } else {
          // Item selected, remove it from the output
          currentItem.__selected = false;

          output = output.filter((num) => num !== currentItem[props.itemValue]);

          if (needToCheckChildren) {
            // Remove children from the output and push them to the stack
            currentItem.children.forEach((child) => {
              child.__selected = true;
              stack.push(child);
            });
          }
        }
      }
      updateModelValue();
    }

    function selectAll(rawArray) {
      output = [];
      rawArray.forEach((item) => {
        item.__selected = true;
        output.push(item[props.itemValue]);
      });
      updateModelValue();
    }

    function deleteAll(rawArray) {
      rawArray.forEach((item) => {
        item.__selected = false;
        item.__expanded = false;
      });
      output = [];
      updateModelValue();
    }

    onMounted(() => addSelectedItems(props.rawData))

    return {
      tree,
      updateOutput,
      selectAll,
      deleteAll
    };
  }
}
</script>
