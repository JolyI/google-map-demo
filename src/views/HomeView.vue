<script setup lang="ts"></script>

<template>
  <el-row :gutter="20">
    <el-col :span="12" :offset="0">
      <el-card shadow="hover" :body-style="{ padding: '0px' }">
        <template #header>
          <div>
            <span>marker</span>
          </div>
        </template>
        <!-- card body -->
        <div>标记</div>
      </el-card>
    </el-col>
  </el-row>
</template>
