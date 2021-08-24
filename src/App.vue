<template>
  <div  style="border: 1px solid #ccc; margin-top: 10px;height: 500px">
    <Editor :editorId="editorId"  :defaultConfig="defaultConfig" :defaultContent="defaultContent" @onCreated="handleCreated"/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Editor from './components/Editor.vue'
import { SlateEditor } from '@wangeditor/editor-cattle'

export default defineComponent({
  name: 'App',
  data(){
    return{
      defaultContent: [{ type: 'paragraph', children: [{ text: 'basic demo' }] }],
      defaultConfig: {},
      mode:'default',
      editorId: 'we-1001'
    }
  },
  components: {
    Editor
  },
  methods:{
    handleCreated(editor: SlateEditor){
      console.log('%c 🍸 editor: ', 'font-size:20px;background-color: #465975;color:#fff;', editor);
    }
  }
})
</script>


