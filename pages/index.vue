<!--eslint-disable-->
<template>
      <el-form label-position="left" label-width="152px" ref="form" class="novel-form" :model="formData" :rules="rules">
        <el-form-item label="标签" prop="tag" class="form-tag">
          <el-select
              v-model="formData.tag"
              clearable
              multiple
              filterable
              remote
              allow-create
              default-first-option
              :multiple-limit="20"
              :popper-append-to-body="false"
              placeholder=""
          >
            <el-option v-for="item in tagList" :key="item" :value="item" >
              <span v-html="item"></span>
            </el-option>
          </el-select>
        </el-form-item>
      </el-form>
</template>
<script>
/* eslint-disable */
import Vue from 'vue';
import { Form, FormItem, Select, Option } from 'element-ui';
Vue.use(Form).use(FormItem).use(Select).use(Option);

export default {
  name: "test",
  data() {
    // 标签自定义校验方法
    const customValidateTag = (rule, value, callback) => {

      const limit = 15;
      value.forEach((tag, index) => {
        const regChinese = /[\u4e00-\u9fa5]/g; // 匹配中文正则
        let length = 0;
        for (let i = 0; i < tag.length; i++) {
          const str = tag.charAt(i);
          const addLength = str.match(regChinese) ? 1 : 0.5;
          length += addLength;
        }

        if (length > limit) {
          callback(new Error('* 单个标签最多只支持'+ limit +'个字符'));
        }
      });
      callback();
    };
    return {
      rules: {
        tag: [
          { required: true, message: '* 请填写标签', trigger: 'change' },
          { validator: customValidateTag, trigger: 'change' }
        ]
      },
      formData: {
        tag: []
      },
      tagList: ['A', 'B', 'C']
    }
  },
  fetch() {}
}
</script>

<style lang="scss" scoped>

</style>
