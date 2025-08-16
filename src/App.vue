<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="common-layout">
    <el-container>
      <el-header>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/treeOperate">TreeOperate</RouterLink>
      </el-header>
      <el-main>
        <RouterView />
      </el-main>
    </el-container>
  </div>
</template>

<style scoped>
.common-layout {
  height: 100%;
  width: 100%;
}
</style>
