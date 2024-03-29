<template>
  <div ref="box" class="w-e-box-init"></div>
</template>

<script lang='ts'>
import {onMounted,defineComponent, reactive, ref, PropType,  } from 'vue'
import { createEditor, IEditorConfig,SlateEditor } from '@wangeditor/editor-cattle'
import {Descendant} from 'slate';
import emitter from '../utils/emitter'
import { recordEditor } from '../utils/editor-map'
import {genErrorInfo} from '../utils/cteate-info'

export default defineComponent({
  props:{
    /** 编辑器默认ID */
    editorId:  {
      type: String,
      required: true,
    },
    /** 编辑器模式 */
    mode: {
      type: String,
      default: 'default'
    },
    /** 编辑器默认内容 */
    defaultContent: {
      type: Array as PropType<Descendant[]>,
      default: []
    },
    /** 编辑器默认配置 */
    defaultConfig: { 
      type: Object as PropType<IEditorConfig>,
      default: {}
    },
  },
  setup(props,context) {
    // 编辑器容器
    const box = ref(null)
    /**
     * 初始化编辑器
     */
    const initEditor = () => {
      if(!box.value) return 
      createEditor({
        textareaSelector: (box.value!) as Element,
        mode: props.mode,
        content: props.defaultContent ,
        config: {
          ...props.defaultConfig,
            onCreated(editor) {
            // 记录 editor
            recordEditor(props.editorId, editor)

            // 触发自定义事件（如创建 toolbar）
            emitter.emit(`w-e-created-${props.editorId}`)

            context.emit('onCreated', editor)
            if (props.defaultConfig.onCreated) {
              const info = genErrorInfo('onCreated')
              throw new Error(info)
            }
          },
          onChange(editor) {
            context.emit('onChange', editor)
            if (props.defaultConfig.onChange) {
              const info = genErrorInfo('onChange')
              throw new Error(info)
            }
          },
          onDestroyed(editor) {
            context.emit('onDestroyed', editor)
            if (props.defaultConfig.onDestroyed) {
              const info = genErrorInfo('onDestroyed')
              throw new Error(info)
            }
          },
          onMaxLength (editor) {
            context.emit('onMaxLength', editor)
            if (props.defaultConfig.onMaxLength) {
              const info = genErrorInfo('onMaxLength')
              throw new Error(info)
            }
          },
          onFocus(editor) {
            context.emit('onFocus', editor)
            if (props.defaultConfig.onFocus) {
              const info = genErrorInfo('onFocus')
              throw new Error(info)
            }
          },
          onBlur(editor) {
            context.emit('onBlur', editor)
            if (props.defaultConfig.onBlur) {
              const info = genErrorInfo('onBlur')
              throw new Error(info)
            }
          },
          customAlert (info, type) {
            context.emit('customAlert', info, type)
            // @ts-ignore
            if (props.defaultConfig.customAlert) {
              const info = genErrorInfo('customAlert')
              throw new Error(info)
            }
          },
        }
      })
    }

    onMounted(() => {
      initEditor()
    })

    return {
      box,
    }
  },
})
</script>

<style>
@import url(@wangeditor/editor-cattle/dist/css/style.css);
.w-e-box-init {
  height: 100%;
}
</style>
