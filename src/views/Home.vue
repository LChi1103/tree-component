<template>
  <div class="home">
    <tree
      :nodeList="nodeList"
      :clickNode="clickNode"
      :activeIndex="activeIndex"
      >
      <template v-slot="content">
        <div>{{ content.node.index }}</div>
      </template>
    </tree>
  </div>
</template>

<script>
import Tree from '../components/Tree'
export default {
  name: 'Home',
  components: {
    Tree
  },
  data () {
    return {
      activeIndex: '',
      nodeList: [
        {
          index: '1',
          name: '树结构-1',
          children: [
            {
              index: '1-1',
              name: '树结构-1-1',
            },{
              index: '1-2',
              name: '树结构-1-2',
              children: [
                {
                  index: '1-2-1',
                  name: '树结构-1-2-1',
                },{
                  index: '1-2-2',
                  name: '树结构-1-2-2',
                }
              ]
            }
          ]
        },{
          index: '2',
          name: '树结构-2',
          children: [
            {
              index: '2-1',
              name: '树结构-2-1',
            }
          ]
        },{
          index: '3',
          name: '树结构-3',
        }
      ]
    }
  },
  methods: {
    clickNode (node, index) {
      console.log('clickNode')
      console.log(node)
      console.log(index)
      this.activeIndex = node.index
    }
  }
}
</script>

<style scoped>
.home {
  width: 500px;
  margin: 50px auto;
  padding: 15px;
  box-shadow: 0 0 10px #eee;
}
</style>
