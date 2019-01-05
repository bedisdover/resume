<template>
  <div class="editor">
    <div class="header">
      <div>
        <el-upload
          action="/"
          accept="*.json"
          :auto-upload="false"
          :multiple="false"
          :show-file-list="false"
          :on-change="load">
          <el-button slot="trigger" type="primary">选择简历</el-button>
          <span class="name">{{ name }}</span>
        </el-upload>
      </div>

      <div>
        <el-button type="primary" plain @click="save">保存</el-button>
        <el-button plain @click="print">打印</el-button>
      </div>
    </div>

    <module title="基本信息">
      <div class="input-line">
        <div class="input-label">姓名</div>
        <el-input v-model="resume.name"></el-input>
      </div>
      <div class="input-line">
        <div class="input-label">手机号</div>
        <el-input v-model="resume.phone"></el-input>
      </div>
      <div class="input-line">
        <div class="input-label">邮箱</div>
        <el-input v-model="resume.email"></el-input>
      </div>
      <div class="input-line">
        <div class="input-label">城市</div>
        <el-input v-model="resume.city"></el-input>
      </div>
      <div class="input-line">
        <div class="input-label">微信</div>
        <el-input v-model="resume.wechat"></el-input>
      </div>
      <div class="input-line">
        <div class="input-label">求职意向</div>
        <el-input v-model="resume.intention"></el-input>
      </div>
      <div class="input-line">
        <div class="input-label">头像</div>
        <el-upload
          class="avatar-uploader"
          action="/"
          :auto-upload="false"
          :multiple="false"
          :show-file-list="false"
          :on-change="uploadAvatar">
          <img v-if="resume.avatar" :src="resume.avatar" class="avatar">
          <i v-else
             slot="trigger"
             class="el-icon-plus avatar-uploader-icon"></i>
        </el-upload>
      </div>
    </module>

    <module
      v-for="(section, sectionID) in resume.content"
      :key="sectionID"
      :title="section.name">
      <template slot="operation" class="operation">
        <el-button
          v-if="sectionID !== 0"
          type="primary"
          icon="el-icon-arrow-up"
          circle
          plain
          size="small"
          @click="moveUp(resume.content, sectionID)"></el-button>
        <el-button
          v-if="sectionID !== resume.content.length - 1"
          type="primary"
          icon="el-icon-arrow-down"
          circle
          plain
          size="small"
          @click="moveDown(resume.content, sectionID)"></el-button>
        <el-button
          type="danger"
          icon="el-icon-delete"
          circle
          size="small"
          @click="remove(resume.content, sectionID)"></el-button>
      </template>

      <div class="input-line">
        <div class="input-label">模块名称</div>
        <el-input v-model="section.name"></el-input>
      </div>
      <div class="input-line">
        <div class="input-label">模块图标</div>
        <icon-selector v-model="section.icon"></icon-selector>
      </div>
      <div class="input-line">
        <div class="input-label">模块描述</div>
        <el-input v-model="section.desc" type="textarea" autosize></el-input>
      </div>
      <div class="item" v-for="(item, itemID) in section.items" v-bind:key="itemID">
        <div class="input-line">
          <div class="input-label">项目名称</div>
          <el-input v-model="item.object"></el-input>
          <div class="operation">
            <el-button
              v-if="itemID !== 0"
              type="primary"
              icon="el-icon-arrow-up"
              circle
              plain
              size="mini"
              @click="moveUp(section.items, itemID)"></el-button>
            <el-button
              v-if="itemID !== section.items.length - 1"
              type="primary"
              icon="el-icon-arrow-down"
              circle
              plain
              size="mini"
              @click="moveDown(section.items, itemID)"></el-button>
            <el-button
              type="danger"
              icon="el-icon-delete"
              circle
              size="mini"
              @click="remove(section.items, itemID)"></el-button>
          </div>
        </div>
        <div class="input-line">
          <div class="input-label">时间</div>
          <el-input v-model="item.time"></el-input>
        </div>
        <div class="input-line">
          <div class="input-label">岗位 / 职责</div>
          <el-input v-model="item.subject"></el-input>
        </div>
        <div class="input-line">
          <div class="input-label">项目描述</div>
          <el-input v-model="item.desc" type="textarea"></el-input>
        </div>
        <div class="content input-line">
          <div class="input-label">事项列表</div>
          <el-input v-model="item.contents" type="textarea" :autosize="{minRows: 4}"></el-input>
        </div>
      </div>

      <el-button type="primary" size="small" @click="addItem(sectionID)" class="add">添加项目</el-button>
    </module>

    <el-button type="primary" @click="addSection" class="add">添加模块</el-button>
  </div>
</template>

<script>
import Module from './Module'
import IconSelector from './IconSelector'

export default {
  name: 'Editor',

  props: [
    'name',
    'resume'
  ],

  components: {
    Module,
    IconSelector
  },

  methods: {
    load (file) {
      this.$emit('onLoad', file.raw)
    },
    save () {
      this.$emit('onSave')
    },
    print () {
      this.$emit('onPrint')
    },
    uploadAvatar (f) {
      const file = f.raw
      const isImage = file.type.startsWith('image/')
      const isLt2M = file.size / 1024 / 1024 < 2

      if (!isImage) {
        this.$message.error('上传头像必须为图片!')
        return
      }
      if (!isLt2M) {
        this.$message.error('图片大小不能超过 2MB!')
        return
      }

      const reader = new FileReader()
      const _this = this
      reader.onload = function () {
        _this.$set(_this.resume, 'avatar', this.result)
      }
      reader.readAsDataURL(file)
    },
    addSection () {
      this.resume.content.push({
        name: '',
        icon: '',
        desc: '',
        items: []
      })
    },
    moveUp (arr, index) {
      const before = arr[index - 1]
      const after = arr[index]
      this.$set(arr, index - 1, after)
      this.$set(arr, index, before)
    },
    moveDown (arr, index) {
      const before = arr[index]
      const after = arr[index + 1]
      this.$set(arr, index, after)
      this.$set(arr, index + 1, before)
    },
    remove (arr, id) {
      arr.splice(id, 1)
    },
    addItem (sectionID) {
      this.resume.content[sectionID].items.push({
        object: '',
        time: '',
        subject: '',
        desc: '',
        contents: ''
      })
    }
  }
}
</script>

<style scoped lang="scss">
  .editor {
    width: 50%;
    height: 100vh;

    .header {
      margin: 10px;
      display: flex;
      justify-content: space-between;
    }

    section {
      border: solid 1px;
      border-radius: 5px;
      margin: 10px;
    }

    .add {
      margin: 10px;
    }

    .operation {
      display: inline-flex;
      float: right;
      margin-right: 10px;
    }

    .input-line {
      margin: 10px 0;

      .input-label {
        display: inline-block;
        width: 100px;
        line-height: 40px;
        text-align: right;
        margin-right: 20px;
        vertical-align: top;
      }

      .el-input {
        width: 200px;
      }

      .el-textarea {
        width: calc(100% - 130px)
      }

      .avatar-uploader {
        display: inline-block;

        .avatar-uploader-icon {
          font-size: 28px;
          color: #8c939d;
          width: 178px;
          height: 178px;
          line-height: 178px;
          text-align: center;
          border: 1px dashed;
          border-radius: 6px;
          cursor: pointer;

          &:hover {
            border-color: #409EFF;
          }
        }

        .avatar {
          width: 178px;
          height: 178px;
          display: block;
        }
      }
    }

    .item {
      background: #eeeeee;
      padding: 10px 0;
      margin: 10px;
      border-radius: 4px;
    }
  }
</style>
