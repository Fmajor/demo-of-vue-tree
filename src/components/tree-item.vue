<template>
  <div class="tree-item">
    <template v-if="tree.root">
      <div ref="subtree" v-if="tree.children" class="subtree">
        <ac-tree-item
           v-for="child of tree.children"
          :key="child.path"
          :tree="child"
          :level="level+1"
        />
      </div>
    </template>
    <template v-else>
      <template v-if="tree.children">
        <div class="node-content">
          <span>{{tree.name}}</span>
          <button type="button" name="button" @click="changeStyle">changeStyle</button>
          <button type="button" name="button" @click="changeValueInVShow">changeValueInVShow</button>
        </div>
        <div ref="subtree" class="subtree" v-show="status.open" style="display:unset;">
          <ac-tree-item
           v-for="child of tree.children"
           :key="child.path"
           :tree="child"
           :level="level+1"
          />
        </div>
      </template>
      <template v-else> <!--not root and not have children-->
        <div class="node-content">
          <span>{{tree.name}}</span>
        </div>
      </template>
    </template>
  </div>
</template>

<script>
export default {
  name: 'tree-item',
  props: {
    tree: { type: Object, default () { return {} } },
    level: { type: Number, default:0 },
    treeState: { type: Object, default () { return {} } },
  },
  data () {
    return {
      status: {
        open: false
      }
    }
  },
  computed: {
  },
  mounted () {
  },
  watch: {
  },
  created () {
    this.updateFold()
    this.$watch('tree', value => {
      console.log(`${this.tree.path}: tree updated`)
      this.updateFold() // update when we change tree from the top level
    })
  },
  methods: {
    updateFold (value) {
      if (value===undefined) {
        this.status.open = this.tree.status.open
        //this.status.open = true
        //console.log(`${this.tree.path} init open to`, this.status.open)
      } else {
        console.log(`${this.tree.path} change open to`, value)
        this.tree.status.open = value
        this.status.open = value
      }
    },
    changeValueInVShow (event) {
      if (this.tree.children) {
        this.status.open = !this.status.open // change local value
        this.updateFold(this.status.open)
      }
    },
    changeStyle (event) {
      console.log('changeStyle', this.$refs.subtree)
      let el = this.$refs.subtree
      let current = el.style.getPropertyValue('display')
      if (current === 'none') {
        el.style.setProperty('display', 'unset')
      } else {
        el.style.setProperty('display', 'none')
      }
    },
  },
  beforeCreate: function () {
    this.$options.components.acTreeItem = require('./tree-item.vue').default
  },
}
</script>

<style>
.tree-item {
  position: relative;
  left: 1rem;
  margin-right: 1rem;
}
.node-content {

}
.subtree {
  position: relative;
}
</style>
