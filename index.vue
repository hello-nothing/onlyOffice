<template>
  <div class="index" v-loading="loading">
    <editDocu
      :drawer-open="drawerOpen"
      :file-info="fileInfo"
      @close-draw="closeDraw"
      v-if="initThis"
    ></editDocu>
  </div>
</template>

<script>
import api from '../api/api';
import { upload_url } from '../api/index';
import editDocu from '@/components/rd-table.vue';
export default {
  name: 'index',
  components: {
    editDocu
  },
  data() {
    return {
      loading: true,
      drawerOpen: false,
      initThis: false
    };
  },
  mounted() {
    this.getDirectory(0);
    // this.getProjectPeople();
  },
  watch: {
    menuTree() {
      this.initThis = false;
      this.$nextTick(() => {
        this.initThis = true;
      });
    }
  },
  methods: {
    //关闭抽屉
    closeDraw() {
      this.drawerOpen = false;
    },
    // 查看文件
    lookFile(item) {
      this.fileInfo = item;
      this.drawerOpen = true;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
</style>
