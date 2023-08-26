<template>
  <span
    v-if="hasChildren"
    class="tree-node-item-icon"
  >
    <chevron-down v-if="!showChildren" width="10" />
    <chevron-right v-else width="10" />
  </span>
</template>

<script>
import ChevronRight from './Icons/Chevron/Down.vue';
import ChevronDown from './Icons/Chevron/Right.vue';

export default {
    name: 'ToggleIcon',
    components: { ChevronRight, ChevronDown },
    props: {
        hasChildren: {
            type: Boolean,
            default: false
        },
        showChildren: {
            type: Boolean,
            default: false
        },
        changeColorChevron: {
            type: Boolean,
            default: false
        }
    }
};
</script>

<style scoped>
.tree-node-item-icon {
    position: absolute;
    margin-left: -15px;
    margin-top: 7px;
}
</style>
