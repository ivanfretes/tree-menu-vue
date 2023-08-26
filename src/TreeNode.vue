<template>
  <div
    :class="[mainGroupActiveClass]"
  >
    <div
      :class="['tree-node tw-overflow-x-hidden', activeClass, {'first-depth': depth === 1}]"
      :style="indent"
      @click="onClick(nodeId)"
    >
      <ToggleIcon :has-children="nodes.length > 0" :show-children="showChildren" />
      {{ $stringHelpers.capitalize(label) }}
    </div>
    <div>
      <tree-node
        v-for="node in nodes"
        v-if="showChildren"
        :key="node.id"
        :node-id="node.id"
        :nodes="node.children"
        :label="node.name"
        :depth="depth + 1"
        :opened-ids="openedIds"
        :active-id="activeNode"
      />
    </div>
  </div>
</template>

<script>
// import IconChildren from './IconChildren.vue';
import ToggleIcon from './ToggleIcon.vue';

export default {
    name: 'TreeNode',
    components: { ToggleIcon },
    props: {
        nodes: {
            type: Array,
            default () {
                return [];
            }
        },
        label: {
            type: String,
            default: ''
        },
        depth: {
            type: Number,
            default: 0
        },
        nodeId: {
            type: Number,
            default: 0
        },
        hasChildren: {
            type: Boolean,
            default: false
        },
        openedIds: {
            type: Array,
            default () {
                return [];
            }
        },
        activeId: {
            type: Number,
            default () {
                return -1;
            }
        }
    },
    data () {
        const activeGroup = this.openedIds[0] || this.activeId;
        return {
            showChildren: false,
            // Path of Ids selected
            path: this.openedIds,
            // Ids of group and node selected
            activeGroup,
            activeNode: this.activeId
        };
    },
    computed: {
        indent () {
            return { paddingLeft: `${this.depth * 20}px` };
        },
        mainGroupActiveClass () {
            return { 'main-group-active': this.activeGroup === this.nodeId && this.depth === 1 };
        },
        activeClass () {
            return {
                'active-class': this.activeId === this.nodeId && this.depth > 0
            };
        }
    },
    watch: {
        activeId (newVal) {
            this.activeNode = newVal;
        },
        openedIds (newVal) {
            this.activeGroup = newVal[0] || this.activeId;
        }
    },
    mounted () {
        this.openChildren();
    },
    destroyed () {
        this.showChildren = false;
    },
    methods: {
        onClick (nodeId) {
            this.showChildren = !this.showChildren;
            const basePath = this.$route.path.split('/')[1];
            this.$router.push({ path: `/${basePath}/${nodeId}#content` });
        },
        openChildren () {
            if (this.path.includes(this.nodeId)) {
                this.showChildren = true;
            }
        }
    }
};
</script>
