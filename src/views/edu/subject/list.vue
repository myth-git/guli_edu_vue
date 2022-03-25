<template>
  <div class="app-container">
    <el-input
      v-model="filterText"
      placeholder="Filter keyword"
      style="margin-bottom: 30px"
    />
    <!-- 
        ref="tree2"：表示检索id
        :data="data2" 表示要显示的内容
        :props="defaultProps" 表示节点显示
        :filter-node-method="filterNode" 表示检索功能

     -->
    <el-tree
      ref="tree2"
      :data="data2"
      :props="defaultProps"
      :filter-node-method="filterNode"
      class="filter-tree"
      default-expand-all
    />
  </div>
</template>

<script>
import subject from "@/api/edu/subject";
export default {
  data() {
    return {
      filterText: "",
      data2: [], //返回所有分类数据
      defaultProps: {
        children: "children",
        label: "title",
      },
    };
  },
  //页面加载时渲染
  created() {
    this.getAllSubjectList();
  },
  watch: {
    filterText(val) {
      this.$refs.tree2.filter(val);
    },
  },

  methods: {
    getAllSubjectList() {
      subject.getSubjectList().then((response) => {
        this.data2 = response.data.list;
      });
    },
    filterNode(value, data) {
      if (!value) return true;
      return data.title.toLowerCase().indexOf(value) !== -1;
    },
  },
};
</script>

