<template>
  <div :class="`${prefixCls}`">
    <div :class="`${prefixCls}-tools`">
      <span
        :class="`${prefixCls}-tools-button`"
        @click="onChangeAllFold(1)"
      > + </span>
      <span
        :class="`${prefixCls}-tools-button`"
        @click="onChangeAllFold(0)"
      > - </span>
    </div>
    <div :class="`${prefixCls}-content`">
      <ac-tree-item :tree="tree" :treeState="treeState" @update="onupdate"/>
    </div>
  </div>
</template>

<script>
const prefixCls = 'ac-table-tree'
import acTreeItem from './ac-tree-item'

export default {
  name: 'ac-table-tree',
  components: {acTreeItem},
  props: {
    tree: { type: Object, default () { return {} } }
  },
  data () {
    return {
      prefixCls,
      treeState: {
        selected: ''
      },
    }
  },
  computed: {
  },
  methods: {
    onupdate (change, value) {
      //if (change.treeState&&change.treeState.selected) {
      //  this.$set(this.treeState, 'selected', change.treeState.selected)
      //}
      this.$emit('update', change, value)
    },
    goThrough(root, func) {
      func(root)
      if (root.children) {
        for (let child of root.children) {
          this.goThrough(child, func)
        }
      }
    },
    doForAllSubTree (root, func) {
      let children = root.$children.filter(_ => _.$options.name === 'ac-table-tree-item')
      func(root)
      if (children) {
        for (let child of children) {
          this.doForAllSubTree(child, func)
        }
      }
    },
    onChangeAllFold(status) {
      let root = this.$children.find(_ => _.$options.name === 'ac-table-tree-item')
      if (status) {
        this.doForAllSubTree(root, _=> {
          if (_.tree.root) return
          if (_.tree.status.open===false) {
            _.tree.status.open = true
            _.$forceUpdate()
          }
        })
      } else {
        this.doForAllSubTree(root, _=> {
          if (_.tree.root) return
          if (_.tree.status.open===true) {
            _.tree.status.open = false
            _.$forceUpdate()
          }
        })
      }
    }
  }
}
</script>

<style lang="scss">
$pre: ac-table-tree;
.#{$pre} {
}
.#{$pre}-tools {
  height: 1rem;
}
.#{$pre}-tools-button {
  padding: 0 0.5rem;
}
.#{$pre}-tools-button:hover {
  background: white;
}
.#{$pre}-content {
  padding: 0 5px;
}

</style>
