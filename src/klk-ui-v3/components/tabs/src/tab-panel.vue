<template>
  <div
    v-show='active'
    class='klk-tab-panel'
  >
    <slot></slot>
  </div>
</template>
<script lang='ts'>
import { computed, inject, onMounted, onBeforeUnmount, ref } from "vue";
import { rootTabs, RootTabs } from "./tabs.vue";
export default {
  name: "klk-tab-panel",
  props: {
     name: {
      type: String,
      default: '',
    },
  },
  setup(props, ctx) {
    const rootValue = inject<RootTabs>(rootTabs);

    let active = computed(() => {
        return props.name === rootValue.curValue.value;
      }),
      panel = {
        ...props,
        active,
      };

    onMounted(() => {
      rootValue.onPanelAdd(panel);
    });

    onBeforeUnmount(() => {
      rootValue.onPanelRemove(panel);
    });

    return {
      active,
    };
  },
};
</script>