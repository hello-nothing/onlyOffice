<template>
  <el-drawer
    :visible="drawerOpen"
    direction="ttb"
    size="100%"
    :before-close="closeDrawer"
    :destroy-on-close="true"
    @opened="showOnline"
  >
    <div id="onlineEdit"></div>
  </el-drawer>
</template>

<script>
import { mapGetters } from 'vuex';
import { imgUrl } from '../api/index';
export default {
  name: 'editDocu',
  props: {
    drawerOpen: {
      type: Boolean,
      default: false
    },
    fileInfo: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      option: {
        document: {
          fileType: '',
          permissions: {
            comment: true,
            download: false,
            edit: false,
            copy: false,
            print: false
          },
          key: '',
          title: '',
          url: ''
        },
        editorConfig: {
          lang: 'zh-CN',
          // 回调接口，用于编辑后实现保存的功能,(关闭页面5秒左右会触发)
          callbackUrl: '',
          user: {
            id: '',
            name: ''
          }
        },
        token: '',
        height: '100%',
        width: '100%'
      },
      docEditor: null,
      token: ''
    };
  },
  methods: {
    //关闭本抽屉组件
    closeDrawer() {
      this.docEditor.destroyEditor();
      this.$emit('close-draw');
    },
    //实例化在线编辑
    setEditor() {
      // this.option.editorConfig.user = {
      //   id: this.userInfo.ID + '',
      //   name: this.userInfo.userName
      // };
      this.option.token = this.token;
      this.docEditor = new DocsAPI.DocEditor('onlineEdit', this.option);
    },
    //开始动画结束后回调函数
    showOnline() {
      this.setEditor();
    }
  },
  watch: {
    // todo:这里下周简化
    fileInfo: {
      handler: function(val) {
        console.log(val);
        this.option.document.key = val.key;
        this.option.document.title = val.name;
        this.option.document.url = val.url;
        this.option.document.fileType = val.name.split('.')[1];
        this.option.editorConfig.callbackUrl =
          imgUrl + '/rd/document/office/' + val.uuid;
        this.option.editorConfig.user.name = val.author;
        if (val.AllowWrite || val.AllowEdit) {
          this.option.document.permissions.edit = true;
        }
      }
    }
  }
};
</script>

<style scoped>
</style>
