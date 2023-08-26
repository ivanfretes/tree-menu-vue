<template>
  <div class="tree-menu">
    <tree-node
      v-for="node in nodes"
      :key="node.id"
      :nodes="node.children"
      :depth="1"
      :label="node.name"
      :node-id="node.id"
      :opened-ids="opened"
      :active-id="activeId"
    />
  </div>
</template>

<script>
import TreeNode from './TreeNode.vue';
export default {
    name: 'TreeMenu',
    components: { TreeNode },
    props: {
        nodes: {
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
        // array of ids with nodes opened
        return {
            opened: []
        };
    },
    watch: {
        nodes (newVal) {
            if (this.activeId !== -1 && newVal.length) {
                this.setOpened(newVal);
            }
        },
        activeId (newVal, oldVal) {
            if (newVal !== oldVal) {
                this.setOpened(this.nodes);
            }
        }
    },
    methods: {
        getAncestorPath (item, search) {
            const { id, name } = item;
            if (id === search) { return [{ id, name }]; }

            if ((item.children) || Array.isArray(item)) {
                const children = Array.isArray(item) ? item : item.children;
                for (const child of children) {
                    const result = this.getAncestorPath(child, search);
                    if (result) {
                        result.unshift({ id, name });
                        return result;
                    }
                }
            }
        },
        /**
         * Set array of opened Ids
         */
        setOpened (nodes) {
            const ancestors = this.getAncestorPath(nodes, this.activeId);
            if (ancestors) {
                ancestors.shift();
                this.opened =
                    ancestors
                        .filter(item => item.id !== undefined)
                        .map(item => item.id);

                this.$emit('ancestors', ancestors);
            }
        }
    }
};
</script>