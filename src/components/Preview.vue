<template>
  <div class="preview">
    <div class="header">
      <div class="basic-info">
        <div class="name">{{ resume.name }}</div>
        <div class="info">
          {{ [resume.phone, resume.email, resume.city].filter(item => !!item).join(' | ') }}
        </div>
        <div v-if="resume.wechat" class="wechat">
          微信：{{ resume.wechat }}
        </div>
        <div v-if="resume.intention" class="intention">
          求职意向：{{ resume.intention }}
        </div>
      </div>
      <img v-show="resume.avatar" :src="resume.avatar" alt="avatar" class="avatar">
    </div>

    <section v-if="resume.self">
      <div class="section-name">
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-zongjieguanli"></use>
        </svg>
        自我描述
      </div>
      <ul v-for="(item, index) in resume.self.split('\n')" v-bind:key="index" class="content">
        <li>{{ item }}</li>
      </ul>
    </section>

    <section v-for="(section, index) in resume.content" v-bind:key="index">
      <div v-show="section.name" class="section-name">
        <svg class="icon" aria-hidden="true">
          <use :xlink:href="`#icon-${section.icon}`"></use>
        </svg>
        {{ section.name }}
      </div>
      <!--<div v-if="section.desc">-->
        <!--<ul class="content" v-for="(content, index) in section.desc.split('\n')" v-bind:key="index">-->
          <!--<li>{{ content }}</li>-->
        <!--</ul>-->
      <!--</div>-->
      <div class="item" v-for="(item, index) in section.items" v-bind:key="index">
        <div class="object" v-if="item.object">{{ item.subject }} | {{ item.object }}</div>
        <div class="time">{{ item.time }}</div>
        <div class="desc">{{ item.desc }}</div>
        <!--<div v-if="item.achievements">-->
          <!--<div class="responsibility">-->
            <!--<div class="label">职责:</div>-->
            <!--<div class="cont">-->
              <!--<ul v-for="(content, index) in item.contents.split('\n')" v-bind:key="index" class="content">-->
                <!--<li>{{ content }}</li>-->
              <!--</ul>-->
            <!--</div>-->
          <!--</div>-->
          <!--<div class="achievements">-->
            <!--<div class="label">业绩:</div>-->
            <!--<div class="cont">-->
              <!--<ul v-for="(content, index) in item.achievements.split('\n')" v-bind:key="index" class="content">-->
                <!--<li>{{ content }}</li>-->
              <!--</ul>-->
            <!--</div>-->
          <!--</div>-->
        <!--</div>-->
        <div v-if="item.contents.length">
          <ul v-for="(content, index) in item.contents.split('\n')" v-bind:key="index" class="content">
            <li>{{ content }}</li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// import html2canvas from html2canvas

export default {
  name: 'Preview',

  props: ['resume']
}
</script>

<style scoped lang="scss">
.preview {
  object {
    width: 100%;
    height: 100%;
    border: none;
  }

  @font-face {
    font-family: "Adobe Heiti";
    /*src: url("../../static/fonts/AdobeHeitiStd-Regular.otf") format("opentype");*/
  }

  @font-face {
    font-family: "Adobe Songti";
    /*src: url("../../static/fonts/AdobeSongStd-Light.otf") format("opentype");*/
  }

  @font-face {
    font-family: "Adobe Kaiti";
    /*src: url("../../static/fonts/AdobeKaitiStd-Regular.otf") format("opentype");*/
  }

  body {
    padding: 4px 8px 0;
    font-family: "Adobe Songti";
  }

  ul {
    margin: 0;
    padding-left: 20px;
  }

  li {
    line-height: 1.5;
  }

  .iconfont {
    font-weight: normal;
  }

  .header {
    position: relative;
    margin: 25px 0;
  }

  .basic-info {
    font-family: "Adobe Kaiti";
    text-align: center;
  }

  .name {
    font-family: "Adobe Heiti";
    font-size: 24px;
    font-weight: 600;
  }

  img.avatar {
    position: absolute;
    right: 20px;
    top: 0;
    width: 86px;
    height: 86px;
  }

  section {
    margin: 15px 0;
    clear: both;
  }

  .section-name {
    font-family: "Adobe Heiti";
    font-size: 16px;
    font-weight: 600;
    border-bottom: solid 2px;
    margin-bottom: 5px;
  }

  .time {
    float: right;
  }

  .subject {
    font-family: "Adobe Heiti";
    clear: both;
  }

  .desc {
    font-family: "Adobe Kaiti";
    clear: both;
    margin: 5px 0;
  }

  .item:not(:nth-child(2)) {
    margin-top: 10px;
    clear: both;
  }

  .item div:first-child {
    float: left;
  }

  .object {
    font-family: "Adobe Heiti";
    font-weight: bold;
  }

  ul > li.content {
    list-style: none;
    margin-left: -20px;
  }

  div.achievements {
    float: left;
    clear: both;
    margin: 10px 0;
  }

  .label {
    font-family: "Adobe Heiti";
  }

  .cont {
    float: right;
    margin-left: 10px;
  }
}
</style>
