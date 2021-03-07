<template>
  <div class="radio">
    <label class="title">{{ label }}</label>
    <a href="#" v-for="(item, index) in options" :key="index" class="radioLink">
      <div class="radioList">
        <div class="radioArea">
          <label>
            <span>
              <input
                type="checkbox"
                :id="'mchecklist' + index + curTime"
                :value="item.value"
                class="radioInput"
                v-model="checkedVal"
                :disabled="item.disabled || disabled"
              />
              <span
                class="radioSelect"
                :class="
                  typeof item == 'string'
                    ? ''
                    : item.disabled == true
                    ? 'radioSelectDisabled'
                    : ''
                "
              ></span>
            </span>
            <span
              class="selectListItem"
              :for="'mchecklist' + index + curTime"
              >{{ item.name }}</span
            >
          </label>
        </div>
      </div>
    </a>
  </div>
</template>

<script>
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title {
  color: #888;
  font-size: 0.26rem;
  height: 0.6rem;
  line-height: 0.6rem;
  display: block;
  padding: 0rem 0.2rem;
}
.radioLink {
  background-color: #fff;
  box-sizing: border-box;
  color: inherit;
  min-height: 40px;
  display: block;
  overflow: hidden;
  position: relative;
  text-decoration: none;
}

.radioList {
  height: 0.96rem;
  line-height: 0.96rem;
  width: 100%;
  padding: 0rem 0.2rem;
  box-sizing: border-box;
}

.radioArea,
.radioArea label {
  height: 0.96rem;
  width: 100%;
  display: block;
}

.radioInput {
  display: none;
}

.radioSelect {
  box-sizing: border-box;
  display: inline-block;
  background-color: #fff;
  border-radius: 100%;
  border: 1px solid #ccc;
  position: relative;
  width: 20px;
  height: 20px;
  vertical-align: middle;
}

.radioSelectDisabled {
  background-color: #d9d9d9;
  border-color: #ccc;
}

.radioInput:disabled + .radioSelect {
  background-color: #d9d9d9 !important;
  border-color: #ccc !important;
}

.radioInput:checked + .radioSelect {
  background-color: #26a2ff;
  border-color: #26a2ff;
}

.radioInput:checked + .radioSelect::after {
  border-color: #fff;
  -webkit-transform: rotate(45deg) scale(1);
  transform: rotate(45deg) scale(1);
}

.radioSelect::after {
  border: 2px solid transparent;
  border-left: 0;
  border-top: 0;
  content: ' ';
  top: 3px;
  left: 6px;
  position: absolute;
  width: 4px;
  height: 8px;
  -webkit-transform: rotate(45deg) scale(0);
  transform: rotate(45deg) scale(0);
  -webkit-transition: -webkit-transform 0.2s;
  transition: -webkit-transform 0.2s;
  transition: transform 0.2s;
  transition: transform 0.2s, -webkit-transform 0.2s;
}

.selectListItem {
  font-size: 0.3rem;
  vertical-align: middle;
  margin-left: 6px;
}
</style>
