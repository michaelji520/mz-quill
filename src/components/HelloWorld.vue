<template>
  <div class="hello">
    <h1 @click="insertTable">{{ 'HelloWorld' }}</h1>
    <div id="editor"></div>
  </div>
</template>

<script>
import Quill from 'quill';
import QuillTable from 'quill-better-table';

Quill.register({
  'modules/better-table': QuillTable
}, true);
export default {
  name: 'HelloWorld',
  data() {
    return {
      editor: ''
    };
  },
  methods: {
    insertTable() {
      let tableModule = this.editor.getModule('better-table')
      tableModule.insertTable(3, 3)
    }
  },
  mounted() {
    this.editor = new Quill('#editor', {
      theme: 'snow',
      modules: {
        table: false,  // disable table module
        'better-table': {
          operationMenu: {
            items: {
              unmergeCells: {
                text: 'Another unmerge cells name'
              }
            }
          }
        },
        keyboard: {
          bindings: QuillTable.keyboardBindings
        }
      }
    });
  }
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
