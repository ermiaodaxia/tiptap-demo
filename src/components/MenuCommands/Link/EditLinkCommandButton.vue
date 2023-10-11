<template>
  <div>
    <command-button
      :command="openEditLinkDialog"
      :enable-tooltip="enableTooltip"
      :tooltip="t('editor.extensions.Link.edit.tooltip')"
      icon="edit"
    />
<!--:visible.sync="editLinkDialogVisible"-->
    <el-dialog
      :title="t('editor.extensions.Link.edit.control.title')"
      v-model="editLinkDialogVisible"
      :append-to-body="true"
      width="400px"
      custom-class="el-tiptap-edit-link-dialog"
    >
      <el-form :model="linkAttrs" label-position="right" size="small" :rules="rules" ref="formRef">
        <el-form-item
          :label="t('editor.extensions.Link.edit.control.href')"
          prop="href"
        >
          <el-input v-model="linkAttrs.href" autocomplete="off" placeholder="请填写以http或https开头的完整url"/>
        </el-form-item>

        <el-form-item prop="openInNewTab">
          <el-checkbox v-model="linkAttrs.openInNewTab">
            {{ t('editor.extensions.Link.edit.control.open_in_new_tab') }}
          </el-checkbox>
        </el-form-item>
      </el-form>

      <template #footer>
        <el-button size="small" round @click="closeEditLinkDialog">
          {{ t('editor.extensions.Link.edit.control.cancel') }}
        </el-button>

        <el-button
          type="primary"
          size="small"
          round
          @mousedown.prevent
          @click="updateLinkAttrs(formRef)"
        >
          {{ t('editor.extensions.Link.edit.control.confirm') }}
        </el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script lang="ts">
import { defineComponent, inject } from 'vue';
import { Editor } from '@tiptap/vue-3';
import {
  ElDialog,
  ElForm,
  ElFormItem,
  ElInput,
  ElCheckbox,
  ElButton,
  ElMessage
} from 'element-plus';
import CommandButton from '../CommandButton.vue';
import { ref } from 'vue'
export default defineComponent({
  name: 'EditLinkCommandButton',

  components: {
    ElDialog,
    ElForm,
    ElFormItem,
    ElInput,
    ElCheckbox,
    ElButton,
    CommandButton,
  },

  props: {
    editor: {
      type: Editor,
      required: true,
    },

    initLinkAttrs: {
      type: Object,
      required: true,
    },
  },

  setup() {
    const t = inject('t');
    const enableTooltip = inject('enableTooltip', true);
    const formRef = ref()
    return { t, enableTooltip, formRef };
  },

  data() {
    return {
      linkAttrs: this.initLinkAttrs,
      editLinkDialogVisible: false,
      rules:{
        href:[
          {
            required:true,
            message:'链接不能为空',
            trigger:'change'
          },
          {
            pattern:/^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w\.-]+)+[\w\-\._~:/?#[\]@!\$&'\*\+,;=.]+$/,
            message:'请输入以http或https开头的正确链接',
            trigger:'change'
          }
        ]
      }
    };
  },

  methods: {
    updateLinkAttrs(formEl:any) {
      formEl.validate((valid:any, fields:any) => {
        console.log(valid);
        if (valid) {
          this.editor.commands.setLink(this.linkAttrs);
          this.closeEditLinkDialog();
        }
     })
     
    },

    openEditLinkDialog() {
      this.editLinkDialogVisible = true;
    },

    closeEditLinkDialog() {
      this.editLinkDialogVisible = false;
    },
  },
});
</script>
