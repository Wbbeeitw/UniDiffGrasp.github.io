<script setup>
import { ref } from 'vue';
import abstract from '../mds/abstract.mdx';

// Import the images
import image1Src from '@/assets/1.png'; // Assuming you have an alias '@' pointing to 'src/'
import image2Src from '@/assets/2.png';
import logoSrc from '@/assets/logo.png';
// If you don't have the '@' alias, use relative paths:
// import image1Src from '../assets/1.png';
// import image2Src from '../assets/2.png';
// import logoSrc from '../assets/logo.png';


const imageGallery = ref([
  {
    id: 1,
    src: '/1.png', // Use the imported variable
    caption: '图1：项目介绍。'
  },
  {
    id: 2,
    src: '/2.png', // Use the imported variable
    caption: '图2：框架结构图。'
  },
  {
    id: 3,
    src: '/3.png',   // Use the imported variable
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
            <!-- 渲染导入的 abstract 组件 -->
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
.image-item {
  margin-bottom: 24px;
  text-align: center;
}

.image-card {
  border-radius: 8px;
}

.gallery-image {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 0 auto 12px auto;
  border-radius: 4px;
  background-color: #f5f7fa;
}

.caption-container {
  padding: 0 10px;
}

.caption-text {
  font-size: 14px;
  color: #606266;
  line-height: 1.5;
  margin: 0;
  text-align: center;
}

.divider-text {
  font-size: 1.1em;
  color: #909399;
  font-weight: 500;
}

:deep(.el-card__body) {
    padding: 15px;
}
</style>