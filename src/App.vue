<template>
  <div class="root">
    <tree :tree="treeData"/>
  </div>
</template>

<script>
import tree from './components/tree'

function genData(level, maxLevel, index, maxIndex, prefix) {
  let result
  let path
  let name = `${maxLevel-level}-${index}`
  if (prefix) {
    path = `${prefix}.${name}`
  } else {
    path = name
  }
  if (level>0) {
    result = {
      name, path,
      status: {open: true},
      children: [...Array(maxIndex).keys()].map(_ => genData(level-1, maxLevel, _, maxIndex))
    }
  } else {
    result = {
      name, path,
      status: {open: true},
    }
  }
  if (level===0) result.root = true
  return result
}

export default {
  name: 'App',
  data () {
    return {
      treeData: {name:'', path:'', status:{open: false}}
    }
  },
  components: {
    tree
  },
  created () {
    this.treeData = genData(5,5,0,7,'')
    console.log('treeData:', this.treeData)
  }
}
</script>

<style>
.root {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
