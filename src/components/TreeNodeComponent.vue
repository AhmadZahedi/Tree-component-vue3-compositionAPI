<template>
  <li class="d-flex">
    <div
        v-if="node.children"
        class="cursor-pointer icon-wrapper"
        :style="iconContainerStyles"
        @click="toggleExpansion(node)"
    >
      <svg width="20" height="20" viewBox="0 0 16 16">
        <path
            fill="#0989FF"
            d="M12 2a1 1 0 0 0-1.707-.707l-6 6a1 1 0 0 0 0 1.414l6 6a1 1 0 1 0 1.414-1.414L6.414 8l5.293-5.293A1 1 0 0 0 12 2zm0 0"
        />
      </svg>
    </div>

    <input
        :checked="node.__selected"
        type="checkbox"
        :id="node[itemValue]"
        @change="addNodeIdToResult(node)"
    >
    <label :for="node[itemValue]">
      {{ node[itemTitle] }}
    </label>

    <div v-if="!dependent && node.parent_id === null">
      <button
          class="btn btn-success ms-5 fs-1"
          @click="addAllNodesIdToResult"
      >
        Select all
      </button>

      <button
          class="btn btn-danger ms-5 fs-1"
          @click="emptyOutput"
      >
        Delete all
      </button>
    </div>
  </li>

  <ul
      v-if="hasChildren"
      :class="childrenClasses"
      :style="childrenStyles"
      ref="childrenList"
  >
    <TreeNodeComponent
        v-for="child in node.children"
        :key="child[itemValue]"
        :node="child"
        :item-title="itemTitle"
        :item-value="itemValue"
        :dependent="dependent"
        @addNodeIdToResult="addNodeIdToResult"
        @addAllNodesIdToResult="addAllNodesIdToResult"
        @emptyOutput="emptyOutput"
    />
  </ul>
</template>

<script>
import {computed, ref} from "vue";

export default {
  props: {
    node: {
      type: Object,
      required: true
    },
    itemTitle: {
      type: String,
      required: true
    },
    itemValue: {
      type: String,
      required: true
    },
    dependent: {
      type: Boolean
    }
  },

  emits: ['addNodeIdToResult', 'addAllNodesIdToResult', 'emptyOutput'],

  setup(props , { emit }) {
    const childrenList = ref(null);

    //Todo check is this true
    const hasChildren = computed(() => {
      const { children } = props.node;
      return children && children.length > 0;
    });

    const childrenClasses = computed(() => {
      return [
          'children',
          props.node.children ? 'ms-3' : ''
      ];
    });

    const childrenListHeight = computed(() => {
      return childrenList.value?.clientHeight;
    });

    const childrenStyles = computed(() => {
      return {
        height: props.node.__expanded ? '0' : (childrenListHeight.value + 'px')
      };
    });

    const iconContainerStyles = computed(() => {
      return {
        transition: 'transform 200ms ease-in',
        transform: props.node.__expanded ? 'transform: rotate(0deg)' : 'transform: rotate(-90deg)'
      };
    });

    function toggleExpansion(treeNode) {
      console.log(treeNode)
      treeNode.__expanded = !treeNode.__expanded;
    }

    function addNodeIdToResult(item) {
      emit('addNodeIdToResult', item);
    }

    function addAllNodesIdToResult() {
      emit('addAllNodesIdToResult');
    }

    function emptyOutput () {
      emit('emptyOutPut');
    }

    return {
      addNodeIdToResult,
      addAllNodesIdToResult,
      emptyOutput,
      hasChildren,
      childrenClasses,
      childrenStyles,
      iconContainerStyles,
      toggleExpansion
    };
  }
}
</script>

<style scoped>
li {
  list-style: none;
  padding-top: 5px;
}

label {
  user-select: none;
}

.hidden-children {
  height: 0;
}

.children {
  transition: all 200ms ease-in;
  overflow: hidden;
}

.icon-wrapper {
  width: 25px;
  aspect-ratio: 1 / 1;
  border: 1px solid #0989FF;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 5px;
}
</style>
