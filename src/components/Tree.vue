<template>
  <div class="tree">
    <div class="content" v-for="(node, index) in nodes" :key="'node-' + index + '-' + node.index" @click.stop="clickNode(node, index)">
      <!-- 显示的内容 -->
      <div :class="{ 'content-item': true, 'active-content-item': node.index === activeIndex }" :style="paddingLeft">
        <div class="content-control">
          <!-- 箭头 -->
          <template v-if="node.children && node.children.length > 0">
            <img v-if="!node.hide" @click.stop="toggle(node, index)" class="oper-icon" src="@/assets/arrow-1.png" width="16px"/>
            <img v-else @click.stop="toggle(node, index)" class="oper-icon" src="@/assets/arrow-2.png" width="16px"/>
          </template>
          <!-- 内容 -->
          <div>
            <!-- 在这使用插槽 可以自定义节点显示内容 -->
            <slot :node="node">
              <span>{{ node.name }}</span>
            </slot>
          </div>
        </div>
      </div>
      <!-- 子树（有 children ，并且 hide 不为 true 时显示） -->
      <template v-if="node.children && node.children.length > 0 && !node.hide">
        <tree
          :nodeList="node.children"
          :level="level + 1"
          :clickNode="clickNode"
          :activeIndex="activeIndex"
          >
          <!-- 在这使用插槽 可以自定义节点显示内容 (插槽内容传到子节点) -->
          <template v-slot="content">
            <slot :node="content.node">
              <span>{{ content.node.name }}</span>
            </slot>
          </template>
        </tree>
      </template>
    </div>
    <div style="display: none">{{ nodeListChange }}</div>
  </div>
</template>

<script>
import Tree from '../components/Tree'
export default {
  name: 'Tree',
  components: {
    Tree
  },
  props: {
    // 树结构 `{ index, name, children, hide }`
    nodeList: {
      type: Array,
      default () {
        return []
      }
    },
    // 点击 节点 触发的函数 (函数返回 当前节点node 当前节点的index)
    clickNode: {
      type: Function,
      default () {
        return () => {}
      }
    },
    // 当前被选中的 节点
    activeIndex: {
      type: String,
      default: ''
    },
    // 控制左边距
    level: {
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      nodes: []
    }
  },
  computed: {
    nodeListChange () {
      this.getNodes()
      return this.nodeList
    },
    paddingLeft () {
      return { paddingLeft: this.level * 30 + 'px' }
    }
  },
  methods: {
    // 获取可操作树结构
    getNodes () {
      this.nodes = JSON.parse(JSON.stringify(this.nodeList))
    },
    // 显示 & 隐藏 子树
    toggle (node, index) {
      node.hide = !node.hide
      this.$set(this.nodes, index, JSON.parse(JSON.stringify(node)))
    }
  }
}
</script>

<style scoped>
.content-item {
  padding: 15px;
  border-bottom: 1px solid #eee;
  cursor: pointer;
}
.content-item:hover, .active-content-item {
  background-color: #eee;
}
.content-control {
  position: relative;
}
.oper-icon {
  position: absolute;
  top: calc(50% - 8px);
  left: -25px;
}
</style>
