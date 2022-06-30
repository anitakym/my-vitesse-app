<script lang="ts" setup>
import { ref } from 'vue'
import { ElLoading, ElMessage, ElMessageBox } from 'element-plus'

import type { Action, UploadProps, UploadUserFile } from 'element-plus'
const loading = ref(false) // loading
const openLoading = () => {
  loading.value = ElLoading.service({
    lock: true,
    text: '分析中。。。',
    background: 'rgba(0, 0, 0, 0.7)',
  })
}
const closeLoading = () => {
  loading.value.close()
}

const dialogVisible = ref(false)

const openAlert = () => {
  setTimeout(() => {
    dialogVisible.value = true
  }, 1000)
}

const fileList = ref<UploadUserFile[]>([
])

const handleRemove: UploadProps['onRemove'] = (file, uploadFiles) => {
  console.log(file, uploadFiles)
}

const handlePreview: UploadProps['onPreview'] = (uploadFile) => {
  console.log(uploadFile)
}

const handleExceed: UploadProps['onExceed'] = (files, uploadFiles) => {
  ElMessage.warning(
    `The limit is 3, you selected ${files.length} files this time, add up to ${
      files.length + uploadFiles.length
    } totally`,
  )
}

const handleSuccess: UploadProps['onSuccess'] = (files, uploadFiles) => {
  console.log('success')
}

const handleError: UploadProps['onError'] = (files, uploadFiles) => {
  console.log('error')
  openLoading()
  setTimeout(() => {
    closeLoading()
    console.log('ok')
    openAlert()
  }, 1000)
}

const beforeRemove: UploadProps['beforeRemove'] = (uploadFile, uploadFiles) => {
  return ElMessageBox.confirm(
    `Cancel the transfert of ${uploadFile.name} ?`,
  ).then(
    () => true,
    () => false,
  )
}
</script>

<template>
  <div class="middle-part">
    <div v-loading="loading" element-loading-text="分析中..." element-loading-background="rgba(122, 122, 122, 0.8)" class="inner-box">
      <el-upload
        v-model:file-list="fileList"
        class="upload-demo"
        action="https://run.mocky.io/v3/9d059bf9-4660-45f2-925d-ce80ad6c4d15"
        multiple
        :on-preview="handlePreview"
        :on-remove="handleRemove"
        :before-remove="beforeRemove"
        :limit="3"
        :on-exceed="handleExceed"
        :on-success="handleSuccess"
        :on-error="handleError"
      >
        <el-button type="primary">
          Click to upload
        </el-button>
        <template #tip>
          <div class="el-upload__tip">
            请上传视频xxxxx巴拉巴拉
          </div>
        </template>
      </el-upload>
    </div>
    <el-dialog
      v-model="dialogVisible"
      title="结果"
      width="30%"
    >
      <span>您的视频分析结果为不合格</span>
      <template #footer>
        <span class="dialog-footer">
          <el-button type="primary" @click="dialogVisible = false">确认</el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<style scoped>
.middle-part {
  width: 100%;
  height: 100%;
  display: flex;
}
.inner-box {
  width: 704px;
  height: 300px;
  box-shadow: 0px 2px 4px 0px #e5e5e5;
  border-radius: 8px;
  margin: 124px auto 0;
  overflow: hidden;
  background-color: #FFFBEB;
  padding: 50px;
}
</style>

