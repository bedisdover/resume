<template>
  <div class="module" :class="{collapsed: collapse}">
    <section>
      <div class="header">
        <div class="operation">
          <el-tooltip v-if="collapse" content="展开">
            <el-button
              circle
              icon="el-icon-caret-bottom"
              size="small"
              @click="toggle"
            ></el-button>
          </el-tooltip>
          <el-tooltip v-else content="折叠">
            <el-button
              circle
              icon="el-icon-caret-top"
              size="small"
              @click="toggle"
            ></el-button>
          </el-tooltip>

          <slot name="operation"></slot>
        </div>

        <span v-show="collapse" class="title">{{ title }}</span>
      </div>

      <div v-show="!collapse" class="body">
        <slot></slot>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'Module',

  props: ['title'],

  data () {
    return {
      collapse: false
    }
  },

  methods: {
    toggle () {
      this.collapse = !this.collapse
    }
  }
}
</script>

<style scoped lang="scss">
  .module {
    border: solid 1px #606266;
    border-radius: 5px;
    margin: 10px;

    &.collapsed {
      border-color: #ebebeb;
    }

    & .header {
      display: flex;
      flex-direction: row-reverse;
      justify-content: space-between;
      align-items: center;
      padding: 10px;

      & .operation {
        display: inline-flex;
      }

      & .title {
        margin-left: 10px;
      }
    }
  }
</style>
