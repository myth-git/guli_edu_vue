<template>
  <div class="app-container">
    <el-form label-width="120px">
      <el-form-item label="信息描述">
        <el-tag type="info">excel模版说明</el-tag>
        <el-tag>
          <i class="el-icon-download" />
          <a :href="'/static/01.xlsx'">点击下载模版</a>
        </el-tag>
      </el-form-item>
      <!-- 
            ref="upload"   表示组件唯一标识，相当于id
            :auto-upload="false" 表示手动上传
            :disabled="importBtnDisabled" 表示按钮是否隐藏
            :limit="1"  表示只能上传一个文件
            :action="BASE_API + '/eduservice/subject/addSubject'"  表示访问后端请求
            accept="application/vnd.ms-excel"   表示只能上传excel文件
        -->
      <el-form-item label="选择Excel">
        <el-upload
          ref="upload"
          :auto-upload="false"
          :on-success="fileUploadSuccess"
          :on-error="fileUploadError"
          :disabled="importBtnDisabled"
          :limit="1"
          :action="BASE_API + '/eduservice/subject/addSubject'"
          name="file"
          accept="application/vnd.ms-excel.xlsx"
        >
          <el-button slot="trigger" size="small" type="primary"
            >选取文件</el-button
          >
          <el-button
            :loading="loading"
            style="margin-left: 10px"
            size="small"
            type="success"
            @click="submitUpload"
            >上传到服务器</el-button
          >
        </el-upload>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      BASE_API: process.env.BASE_API, // 接口API地址
      importBtnDisabled: false, // 按钮是否禁用,
      loading: false,
    };
  },
  created() {},
  methods: {
    //点击按钮上传文件到接口里面
    submitUpload() {
      this.importBtnDisabled = true;
      this.loading = true;
      // js: document.getElementById("upload").submit()表单提交方式，不能用axios
      this.$refs.upload.submit();
    },
    //上传成功
    fileUploadSuccess(response) {
      //提示信息
      this.loading = false;
      this.$message({
        type: "success",
        message: "添加课程分类成功",
      });
      //跳转课程分类列表
      //路由跳转
      this.$router.push({ path: "/subject/list" });
    },
    //上传失败
    fileUploadError() {
      this.loading = false;
      this.$message({
        type: "error",
        message: "添加课程分类失败",
      });
    },
  },
};
</script>
