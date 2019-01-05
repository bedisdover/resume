<template>
  <el-select v-model="icon" placeholder="请选择">
    <el-option
      v-for="item in icons"
      :key="item.value"
      :value="item.value">
      <div class="item">
        {{ item.label }}
        <svg class="icon" aria-hidden="true">
          <use :xlink:href="`#icon-${item.value}`"></use>
        </svg>
      </div>
    </el-option>
  </el-select>
</template>

<script>
export default {
  name: 'IconSelector',

  model: {
    event: 'changed'
  },

  props: ['value'],

  data () {
    return {
      icons: [
        {label: '社会公益', value: 'shehuigongyi'},
        {label: '技能', value: 'settings'},
        {label: '项目经验', value: 'xiangmujingyan'},
        {label: '教育经历', value: 'jiaoyujingli'},
        {label: '自我描述', value: 'zongjieguanli'},
        {label: '工作经历', value: 'iconlist'},
        {label: '项目经验', value: 'group'}
      ],
      icon: this.value
    }
  },

  watch: {
    icon () {
      this.$emit('changed', this.icon)
    }
  }
}
</script>

<style scoped>
  .item {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
</style>
