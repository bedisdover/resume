<template>
  <div id="app">
    <Editor
      v-if="!preview"
      :name="name"
      :resume="resume"
      @onLoad="handleLoad"
      @onSave="handleSave"
      @onPrint="handlePrint"/>
    <Preview v-if="!preview" :resume="resume" style="height: 100vh"/>
    <Preview v-if="preview" :resume="resume"/>
  </div>
</template>

<script>
import Editor from './components/Editor'
import Preview from './components/Preview'

export default {
  name: 'App',

  components: {
    Editor,
    Preview
  },

  data () {
    return {
      name: '',
      resume: {
        content: []
      },
      preview: false
    }
  },

  methods: {
    save (content, filename) {
      let a = document.createElement('a')
      a.download = filename
      a.style.display = 'none'

      const blob = new Blob([content])
      a.href = URL.createObjectURL(blob)
      document.body.appendChild(a)
      a.click()
      document.body.removeChild(a)
    },
    handleLoad (file) {
      if (file.type !== 'application/json') {
        this.$message.error('简历只能是 json 格式!')
        return
      }

      let reader = new FileReader()
      reader.onload = e => {
        this.name = file.name
        this.resume = Object.assign({}, this.resume, JSON.parse(e.target.result))
      }
      reader.readAsText(file)
    },
    handleSave () {
      if (this.name) { // name 不为空直接存储
        this.save(JSON.stringify(this.resume), this.name)
        return
      }

      this.$prompt('', '另存为', {
        confirmButtonText: '确定',
        cancelButtonText: '取消'
      }).then(({value}) => {
        if (value) {
          if (value.indexOf('.json') === -1) {
            value += '.json'
          }

          this.save(JSON.stringify(this.resume), value)
        }
      })
    },
    handlePrint () {
      this.preview = true

      setTimeout(() => {
        window.print()
        this.preview = false
      }, 500)
    }
  }
}
</script>

<style>
  html, body {
    margin: 0;
    padding: 0;
  }

  .icon {
    width: 1em;
    height: 1em;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;
  }

  #app {
    width: 100%;
    display: flex;
    flex-wrap: nowrap;
    justify-content: space-around;
    overflow: auto;
  }

  #app > div {
    overflow: auto;
    width: 100%;
  }
</style>
