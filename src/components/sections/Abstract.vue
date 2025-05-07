<script setup>
import { ref } from 'vue';
import abstract from '../mds/abstract.mdx';

// 1. 正确导入图片（使用 @/assets/ 路径）
import image1Src from '@/assets/1.png';
import image2Src from '@/assets/2.png';
import image3Src from '@/assets/3.png';
import logoSrc from '@/assets/logo.png';

// 2. 使用导入的变量，而非字符串路径
const imageGallery = ref([
  {
    id: 1,
    src: image1Src, // 直接使用导入的变量
    caption: '图1：项目介绍。'
  },
  {
    id: 2,
    src: image2Src,
    caption: '图2：框架结构图。'
  },
  {
    id: 3,
    src: image3Src,
    caption: '图3：双臂生成抓取姿态原理图。'
  }
]);
</script>

<template>
  <div>
    <!-- 第一部分：摘要 -->
    <el-divider />
    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="16" :lg="12" :xl="12">
        <abstract />
      </el-col>
    </el-row>

    <!-- 第二部分：图片展示 -->
    <el-divider>
      <span class="divider-text">图片展示</span>
    </el-divider>

    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="16" :lg="12" :xl="12">
        <div v-for="image in imageGallery" :key="image.id" class="image-item">
          <el-card shadow="hover" class="image-card">
            <img :src="image.src" :alt="image.caption" class="gallery-image"/>
            <div class="caption-container">
              <p class="caption-text">{{ image.caption }}</p>
            </div>
          </el-card>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
/* 原有样式保持不变 */
.image-item {
  margin-bottom: 24px;
  text-align: center;
}
.gallery-image {
  max-width: 100%;
  height: auto;
}
/* 其他样式... */
</style>
