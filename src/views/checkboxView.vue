<template>
  <div>
    <h1>checkbox</h1>
    <div v-on:click="checkedAll">
      <input type="checkbox" v-model="checkAll" />
      <span>全选</span>
    </div>
    <div v-for="(checkOne, index) in checkList">
      <input
        type="checkbox"
        v-model="checkOne.state"
        v-on:click="checkedOne(checkOne)"
      />
      <span>{{ checkOne.name }}</span>
    </div>

    <div class="about">
      <h1>checkbox components</h1>
      <CheckList
        title="普通多选框列表"
        v-model="value"
        @getData="getinfo"
        :options="options"
      />
      <p>您选中的是：{{ value }}</p>
      <CheckList
        title="有禁用选项的多选框列表"
        v-model="value1"
        @getData="getTtInfo"
        :options="options1"
      />
      <p>您选中的是：{{ value1 }}</p>
      <CheckList
        title="有选中禁用的多选框列表"
        v-model="value2"
        @getData="getinfov"
        :options="options2"
      />
      <p>您选中的是：{{ value2 }}</p>
      <button @click="changeOne">change optionE</button>
      <h1>checkbox components 1</h1>
      <multi-check-list
        label="多选测试1"
        :options="multiCheckOptions"
        v-model="checkedVal"
      ></multi-check-list>
      <p>您选中的是：{{ checkedVal }}</p>
      <h1>checkList2</h1>
      <CheckList2
        label="多选测试1"
        :options="multiCheckOptions1"
        v-model="checkedVal1"
      />
      <p>您选中的是：{{ checkedVal1 }}</p>
    </div>
  </div>
</template>

<script>
import checkList from './checkbox/checkList'
import checkList2 from './checkbox/checkList2'
import MultiCheckList from './checkbox/MultiCheckList'
export default {
  data() {
    return {
      checkList: [
        { state: true, name: '1' },
        { state: false, name: '2' },
        { state: false, name: '3' },
        { state: false, name: '4' },
      ],
      value: '',
      value1: '',
      value2: '',
      options: ['optionA', 'optionB'],
      options1: [
        {
          label: 'disabled option',
          value: 'valueF',
          disabled: true,
        },
        {
          label: 'optionE',
          value: 'valueE',
          disabled: false,
          checked: false,
        },
        {
          label: 'optionA',
          value: 'valueA',
        },
        {
          label: 'optionB',
          value: 'valueB',
        },
      ],
      options2: [
        {
          label: 'checked disabled',
          value: 'valueE',
          disabled: true,
          checked: true,
        },
        {
          label: 'optionA',
          value: 'valueA',
        },
        {
          label: 'optionB',
          value: 'valueB',
        },
      ],
      multiCheckOptions: [
        { value: '1', name: '选项1' },
        { value: '2', name: '选项2(禁用)', disabled: true },
        { value: '3', name: '选项3' },
        { value: '4', name: '选项4' },
      ],
      checkedVal: [1, 3],
      multiCheckOptions1: [
        { value: '1', name: '选项1' },
        { value: '2', name: '选项2' },
        { value: '3', name: '选项3' },
      ],
      checkedVal1: [1, 2, 3],
    }
  },
  components: {
    CheckList: checkList,
    CheckList2: checkList2,
    MultiCheckList: MultiCheckList,
  },
  mounted() {
    var _this = this
    var res = ['1', '2', '3'] //这里是默认的选择之
    this.checkList.forEach(function(one, index) {
      if (res.indexOf(one.name) >= 0) _this.checkList[index].state = true
    })
  },
  methods: {
    checkedOne: function(one) {
      one.state = !one.state
    },
    checkedAll: function() {
      var _this = this
      var checkType = true == this.checkAll ? true : false // 循环中(true == this.checkAll)的结果会发生改变
      this.checkList.forEach(function(one, index) {
        if (true == checkType) {
          // 已经全选 走取消的逻辑
          _this.checkList[index].state = false
        } else {
          // 没有全选 走选中的逻辑
          _this.checkList[index].state = true
        }
      })
    },
    clickLeftIcon() {
      this.$router.back(-1)
    },
    getTtInfo(arr) {
      this.value1 = arr
    },
    getinfo(arr) {
      this.value = arr
    },
    getinfov(arr) {
      this.value2 = arr
    },
    changeOne() {
      this.options = ['optionA', 'optionB', 'optionC']
    },
  },
  computed: {
    checkAll: function() {
      return this.checkRes.length == this.checkList.length
    },
    checkRes: function() {
      var res = []
      this.checkList.forEach(function(one) {
        if (true == one.state) res.push(one.name)
      })
      return res
    },
  },
}
</script>

<style>
.about {
  min-height: 100vh;
  background: #fff;
  box-sizing: border-box;
}
.about p {
  padding: 0rem 0.2rem;
  box-sizing: border-box;
  font-size: 0.28rem;
  margin-bottom: 0.8rem;
}
</style>
