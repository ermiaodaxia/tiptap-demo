<template>
  <div>
    <command-button
      :is-active="editor.isActive('link')"
      :readonly="isCodeViewMode"
      :command="openAddLinkDialog"
      :enable-tooltip="enableTooltip"
      :tooltip="t('editor.extensions.Link.add.tooltip')"
      icon="link"
    />

    <el-dialog
      v-model="addLinkDialogVisible"
      :title="t('editor.extensions.Link.add.control.title')"
      :append-to-body="true"
      width="400px"
      custom-class="el-tiptap-edit-link-dialog"
    >
      <el-form :model="linkAttrs" label-position="right" size="small" :rules="rules" ref="formRef">
        <el-form-item
          :label="t('editor.extensions.Link.add.control.href')"
          prop="href"
        >
          <el-input v-model="linkAttrs.href" autocomplete="off" placeholder="请填写以http或https开头的完整url" />
        </el-form-item>

        <el-form-item prop="openInNewTab">
          <el-checkbox v-model="linkAttrs.openInNewTab">
            {{ t('editor.extensions.Link.add.control.open_in_new_tab') }}
          </el-checkbox>
        </el-form-item>
      </el-form>

      <template #footer>
        <el-button size="small" round @click="closeAddLinkDialog">
          {{ t('editor.extensions.Link.add.control.cancel') }}
        </el-button>

        <el-button
          type="primary"
          size="small"
          round
          @mousedown.prevent
          @click="addLink(formRef)"
        >
          {{ t('editor.extensions.Link.add.control.confirm') }}
        </el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script lang="ts">
import { defineComponent, inject } from 'vue';
import {
  ElDialog,
  ElForm,
  ElFormItem,
  ElInput,
  ElCheckbox,
  ElButton,
} from 'element-plus';
import { Editor } from '@tiptap/core';
import CommandButton from '../CommandButton.vue';
import { ref } from 'vue'
export default defineComponent({
  name: 'AddLinkCommandButton',

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
  },

  setup() {
    const t = inject('t');
    const enableTooltip = inject('enableTooltip', true);
    const isCodeViewMode = inject('isCodeViewMode', true);
    const formRef = ref()
    return { t, enableTooltip, isCodeViewMode,formRef };
  },

  data() {
    return {
      linkAttrs: {
        href: '',
        openInNewTab: true,
      },
      addLinkDialogVisible: false,
      rules:{
        href:[
          {
            required:true,
            message:'链接不能为空',
            trigger:'change'
          },
          {
            pattern:/(http|https):\/\/([\w.]+\/?)\S*/,
            message:'请输入以http或https开头的正确链接',
            trigger:'change'
          }
        ]
      }
    };
  },

  watch: {
    addLinkDialogVisible() {
      this.linkAttrs = { href: '', openInNewTab: true };
    },
  },

  methods: {
    openAddLinkDialog() {
      this.addLinkDialogVisible = true;
    },

    closeAddLinkDialog() {
      this.addLinkDialogVisible = false;
    },

    addLink(formEl:any) {
      formEl.validate((valid:any, fields:any) => {
        if (valid) {
          if (this.linkAttrs.openInNewTab) {
            this.editor.commands.setLink({
              href: this.linkAttrs.href,
              target: '_blank',
            });
          } else {
            this.editor.commands.setLink({ href: this.linkAttrs.href });
          }
          this.closeAddLinkDialog();
        }
     })
    },
  },
});
</script>
