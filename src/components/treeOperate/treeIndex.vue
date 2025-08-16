<template>
  <div class="tree-operate-container">
    <el-row :gutter="20">
      <el-col :span="6">
        <el-button style="width: 100%" type="primary" @click="handleCopyData"
          >获取复制数据</el-button
        >
      </el-col>
      <el-col :span="6">
        <el-select v-model="findItemKey" placeholder="Select" style="width: 100%">
          <el-option
            v-for="item in findItemKeyOptions"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          />
        </el-select>
      </el-col>
      <el-col :span="6">
        <el-input v-model="findValue" style="width: 100%" placeholder="Please input" />
      </el-col>
      <el-col :span="6">
        <el-button style="width: 100%" type="primary" @click="handleTree">处理</el-button>
      </el-col>
    </el-row>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue'
const treeData = ref('')
const findItemKey = ref('')
const findValue = ref('')
const findItemKeyOptions = computed(() => {
  // 如果JSON.parse解析失败
  try {
    const tree = JSON.parse(treeData.value || '[]')
    const firstItme = tree[0]
    const keys = []
    for (const key in firstItme) {
      keys.push({
        value: key,
        label: key,
      })
    }
    return keys
  } catch (error) {
    console.log('JSON.parse解析失败', error)
    return []
  }
})

const findNodeInTree = (tree, value, key = 'id') => {
  for (const node of tree) {
    // 1. 检查当前节点
    if (node[key] === value) {
      return node
    }
    // 2. 如果有子节点，则递归进入子节点查找
    if (node.children && node.children.length > 0) {
      const found = findNodeInTree(node.children, value, key)
      // 3. 如果在子节点中找到了，直接返回结果
      if (found) {
        return found
      }
    }
  }
  // 4. 遍历完所有节点及其后代都没找到，返回 null
  return null
}
const handleTree = () => {
  if (!findItemKey.value || !findValue.value || !treeData.value) {
    return
  }
  // 如果JSON.parse解析失败
  try {
    const tree = JSON.parse(treeData.value || '[]')
    const finditem = findNodeInTree(tree, findValue.value, findItemKey.value)
    console.log(finditem)
    if (finditem) {
      navigator.clipboard.writeText(JSON.stringify(finditem))
    }
  } catch (error) {
    console.log('JSON.parse解析失败', error)
    ElMessage({
      message: '处理失败',
      type: 'error',
    })
    return
  }
}
const handleCopyData = () => {
  navigator.clipboard.readText().then((res) => {
    treeData.value = res
    ElMessage({
      message: '获取成功',
      type: 'success',
    })
  })
}
</script>
<style scoped>
.tree-operate-container {
  width: 100%;
}
</style>
