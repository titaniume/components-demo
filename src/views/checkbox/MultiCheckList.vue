<template>
  <div class="multi-checklist">
    <label class="multi-checklist-title">{{ label }}</label>
    <div
      :class="showbox ? 'multi-checklist-contentc' : 'multi-checklist-content'"
    >
      <div class="option-item" v-for="(item, index) in options">
        <input
          type="checkbox"
          :id="'mchecklist' + index + curTime"
          :value="item.value"
          v-model="checkedVal"
          class="checkbox"
          :disabled="item.disabled || disabled"
        />
        <label :for="'mchecklist' + index + curTime" class="check-label">{{
          item.name
        }}</label>
      </div>
    </div>
  </div>
</template>

<script>
/**
 * @author liuxin
 * @description 多选操作 结果是数组，读取value值
 * @param  label：组件的标签名称
 * @param  options：选项 [{value:'1',name:'选项1',disabled:false}] value:选中的值  name:checkbox显示的名称  disabled:禁用控件，默认不禁用
 * @param  checked：选中的数据 ['1']，通常与disabled:true合并使用
 * @param  disabled：是否全部禁用， 默认false可用
 * @param  showbox：是否显示勾选框， 默认显示 ( false:不显示，则样式会改变)
 */
export default {
  name: 'multi-check-list',
  model: {
    // v-model  对应的变量和事件，默认是value和change
    prop: 'checked',
    event: 'change',
  },
  props: {
    // 组件参数
    label: String,
    options: Array,
    checked: Array,
    disabled: false,
    showbox: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      checkedVal: this.checked, // 用来存储选中的数据
      curTime: new Date().getTime(), // 获取当前时间，生成唯一id，保证一个页面可使用多个组件
    }
  },
  watch: {
    // 监听数据变化，回传数据到父组件
    checkedVal(newValue, oldValue) {
      if (newValue != oldValue) {
        this.$emit('change', newValue)
      }
    },
    checked: {
      handler(newVal, oldVal) {},
      deep: true,
    },
  },
}
</script>
</style>
