<template>
  <li class="mt-2">
    <div
        class="d-flex gap-2 align-items-center"
        :style="node.children.length ? '' : 'padding-left: 8px'"
    >
      <div
          v-show="node.children.length"
          class="icon-container"
          :style="iconContainerStyles"
          @click="toggleExpansion(node)"
      >
        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" fill="none"
             viewBox="0 0 24 24">
          <path fill="#00e1ff" d="M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" opacity=".1"/>
          <path stroke="#00e1ff" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"/>
          <path stroke="#00e1ff" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="m11 15 2.716-2.716v0a.402.402 0 0 0 0-.568v0L11 9"/>
        </svg>
      </div>

      <input
          type="checkbox"
          :id="node[itemValue]"
          :checked="node.__selected"
          class="form-check-input me-1 custom-check"
      >

      <label
          :for="node[itemValue]"
          class="form-check-label"
      >
        {{ node[itemTitle] }}
      </label>
    </div>

    <ul
        v-if="node.children"
        ref="childrenElement"
        class="children-list"
        :style="childrenListStyles"
    >
      <TreeNodeComponent
          v-for="child in node.children"
          :node="child"
          :item-title="itemTitle"
          :item-value="itemValue"
      />
    </ul>
  </li>
</template>

<script>
import {computed, ref} from "vue";

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

  setup(props) {
    const iconContainerStyles = computed(() => {
      return {
        transition: 'transform 0.5s ease',
        transform: props.node.__expanded ? 'rotate(90deg)' : ''
      };
    });

    const childrenElement = ref(null);

    const childrenListHeight = computed(() => {
      return childrenElement.value && childrenElement.value.clientHeight;
    });

    const childrenListStyles = computed(() => {
      return {
        transition: 'height 0.5s ease',
        height: props.node.__expanded ? (childrenListHeight.value + 'px') : '0',
        overflow: 'hidden'
      };
    });

    function toggleExpansion() {
      props.node.__expanded = !props.node.__expanded;
    }

    return {
      iconContainerStyles,
      childrenElement,
      childrenListStyles,
      toggleExpansion
    };
  }

}
</script>

<style scoped>
li {
  list-style: none;
}

.custom-check {
  width: 25px;
  height: 25px;
  margin-top: 0 !important;
}

.icon-container {
  cursor: pointer;
  width: 32px;
  height: 32px;
}
</style>
