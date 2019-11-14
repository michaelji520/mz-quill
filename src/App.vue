<template>
  <div id="app">
    <div>
      <quill-editor
        ref="myTextEditor"
        v-model="content"
        :options="editorOption"
        @blur="onEditorBlur($event)"
        @focus="onEditorFocus($event)"
        @ready="onEditorReady($event)">
      </quill-editor>
      <el-dialog
        title="图片上传"
        class="img-dialog"
        :visible.sync="dialogVisible"
        :before-close="handleClose">
        <el-upload
          class="upload-demo"
          drag
          action="https://jsonplaceholder.typicode.com/posts/"
          :before-upload="beforeImageUpload"
          :limit="20"
          multiple>
          <i class="el-icon-upload"></i>
          <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
          <div class="el-upload__tip" slot="tip">只能上传图片文件，且图片大小不超过2MB</div>
        </el-upload>
        <span slot="footer" class="dialog-footer">
          <el-button @click="handleClose">取 消</el-button>
          <el-button type="primary" @click="handleInsertImage">插 入</el-button>
        </span>
      </el-dialog>
    </div>
    <div>
      <hello-world></hello-world>
    </div>
  </div>
</template>

<script>  
// require styles
import 'quill/dist/quill.core.css';
import 'quill/dist/quill.snow.css';
import 'quill/dist/quill.bubble.css';

import HelloWorld from './components/HelloWorld';

import { quillEditor, Quill } from 'vue-quill-editor';
import { ImageDrop } from 'quill-image-drop-module';
import ImageResize from 'quill-image-resize-module';
import hljs from 'highlight.js';

Quill.register('modules/imageDrop', ImageDrop);
Quill.register('modules/imageResize', ImageResize);

const sizeStyle = Quill.import('attributors/style/size');
sizeStyle.whitelist = ['10px', '12px', '14px', '16px', '18px', '20px', '24px', '30px', '32px', '36px'];
Quill.register(sizeStyle, true);

export default {
  name: 'app',
  data() {
    return {
      dialogVisible: false,
      name: '01-example',
      content: 'Text content',
      editorOption: {
        modules: {
          toolbar: {
            container: [
              ['bold', 'italic', 'underline', 'strike'],
              [{ list: 'ordered' }, { list: 'bullet' }],
              [{ size: [false, '10px', '12px', '14px', '16px', '18px', '20px', '24px', '30px', '32px', '36px'] }],
              [{ header: [false, 1, 2, 3, 4, 5, 6] }],
              [{ font: [] }],
              [{ color: [] }, { background: [] }],
              [{ align: [] }],
              ['clean'],
              ['link', 'image'],
            ],
            handlers: {
              image: () => {
                this.dialogVisible = true;
              },
            },
          },
          syntax: {
            highlight: text => hljs.highlightAuto(text).value,
          },
          imageDrop: true,
          imageResize: true,
        },
      },
    };
  },
  components: {
    quillEditor, HelloWorld
  },
  methods: {
    beforeImageUpload(file) {
      const isImage = /.(bmp|tif|gif|jpeg|jpg|png)$/.test(file.type);
      const isLt2M = file.size / 1024 / 1024 < 2;
      if (!isImage) {
        this.$message.error('上传文件只能是图片格式!');
      }
      if (!isLt2M) {
        this.$message.error('上传图片大小不能超过 2MB!');
      }
      return isImage && isLt2M;
    },
    handleInsertImage() {
      const range = this.$refs.myTextEditor.quill.getSelection();
      this.$refs.myTextEditor.quill.insertEmbed(range && range.index, 'image', 'https://upfile.asqql.com/2009pasdfasdfic2009s305985-ts/2019-5/20195312141796041.gif');
      this.handleClose();
    },
    handleClose() {
      console.log('dialog close');
      this.dialogVisible = false;
    },
    onEditorBlur(editor) {
      console.log('editor blur!', editor);
      console.log('content: ', this.$refs.myTextEditor.quill.getContents());
    },
    onEditorFocus(editor) {
      console.log('editor focus!', editor);
    },
    onEditorReady(editor) {
      console.log('editor ready!', editor);
    },
  },
};
</script>

<style lang="less" scoped>
@deep: ~'>>>';
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.quill-editor {
  text-align: left;
}
.img-dialog @{deep} .el-dialog {
  width: 640px;
}
</style>
