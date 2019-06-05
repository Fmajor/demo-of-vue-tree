<template>
  <div class="tree">
    <div class="tree-content`">
      <tree-item :tree="tree"/>
    </div>
  </div>
</template>

<script>
import treeItem from './tree-item'

export default {
  name: 'tree',
  components: {treeItem},
  props: {
    tree: { type: Object, default () { return {} } }
  },
  data () {
    return {
    }
  },
  computed: {
  },
  methods: {
  }
}
</script>

<style>
.tree {
  width: 600px;
  position: relative;
}
</style>
