<template>
  <view class="index">
    <view>abc</view>
    <text :class="styles['msg-text']">{{ msg }}</text>
    <button @tap="gotoInfo">goto info page</button>
    <button type="primary" loading="true">页面主操作 Loading</button>
    <button type="primary" disabled="true">页面主操作 Disabled</button>
    <view class="button-sp-area" style="width: 50%; margin: 0 auto;">
      <button type="primary" plain="true">按钮</button>
      <button type="primary" disabled="true" plain="true">
        不可点击的按钮
      </button>

      <button type="default" plain="true">按钮</button>
      <button type="default" disabled="true" plain="true">按钮</button>

      <button class="mini-btn" type="primary" size="mini">按钮</button>
      <button class="mini-btn" type="default" size="mini">按钮</button>
      <button class="mini-btn" type="warn" size="mini">按钮</button>
    </view>

    <checkbox-group style="width: 100%; height: 300px;">
      <label
        class="weui-cell weui-check__label"
        v-for="item in checkItems"
        :key="item.value"
      >
        <view class="weui-cell__hd">
          <checkbox :value="item.value" :checked="item.checked" />
        </view>
        <view class="weui-cell__bd">{{ item.name }}</view>
      </label>
    </checkbox-group>

    <editor
      id="editor"
      style="width: 100%; height: 300px;"
      @ready="editorReady"
    ></editor>

    <swiper
      :indicator-dots="true"
      :autoplay="true"
      :interval="2000"
      :duration="500"
    >
      <block v-for="item in background" :key="item">
        <swiper-item>
          <view
            :class="{ [styles['swiper-item']]: true, [styles[item]]: true }"
            >{{ item }}</view
          >
        </swiper-item>
      </block>
    </swiper>

    <!-- not working: movable-area is ignored -->
    <movable-area :class="styles['movable-area']">
      <movable-view direction="horizontal" :class="styles['movable-view']"
        >text</movable-view
      >
    </movable-area>

    <!-- not working: map is not showing -->
    <view
      class="page-section page-section-gap"
      style="width: 100%; height: 300px;"
    >
      <map style="width: 100%; height: 300px;">
        <cover-view class="cover-view">
          <cover-view class="container">
            <cover-view class="flex-wrp" style="flex-direction:row;">
              <cover-view class="flex-item demo-text-1"></cover-view>
              <cover-view class="flex-item demo-text-2"></cover-view>
              <cover-view class="flex-item demo-text-3"></cover-view>
            </cover-view>
          </cover-view>
        </cover-view>
      </map>
    </view>

    <view class="page-section">
      <view class="page-section-title">
        <text>Vertical Scroll\n纵向滚动</text>
      </view>
      <view :class="styles['page-section-spacing']">
        <scroll-view
          scroll-y="true"
          style="height: 300rpx;"
          bindscrolltoupper="upper"
          bindscrolltolower="lower"
          bindscroll="scroll"
          :scroll-into-view="'demo3'"
        >
          <view
            id="demo1"
            :class="styles['scroll-view-item'] + ' ' + styles['demo-text-1']"
            >A</view
          >
          <view
            id="demo2"
            :class="styles['scroll-view-item'] + ' ' + styles['demo-text-2']"
            >B</view
          >
          <view
            id="demo3"
            :class="styles['scroll-view-item'] + ' ' + styles['demo-text-3']"
            >C</view
          >
        </scroll-view>
      </view>
    </view>
    <view class="page-section">
      <view class="page-section-title">
        <text>Horizontal Scroll\n横向滚动</text>
      </view>
      <view :class="styles['page-section-spacing']">
        <scroll-view
          class="scroll-view_H"
          scroll-x="true"
          bindscroll="scroll"
          style="width: 100%"
        >
          <view
            id="demo1"
            :class="styles['scroll-view-item'] + ' ' + styles['demo-text-1']"
            >A</view
          >
          <view
            id="demo2"
            :class="styles['scroll-view-item'] + ' ' + styles['demo-text-2']"
            >B</view
          >
          <view
            id="demo3"
            :class="styles['scroll-view-item'] + ' ' + styles['demo-text-3']"
            >C</view
          >
        </scroll-view>
      </view>
    </view>

    <view class="icon-box">
      <icon class="icon-box-img" type="success"></icon>
      <icon class="icon-box-img" type="success_no_circle"></icon>
      <icon class="icon-box-img" type="info"></icon>
      <icon class="icon-box-img" type="warn"></icon>
      <icon class="icon-box-img" type="waiting"></icon>
      <icon class="icon-box-img" type="cancel"></icon>
      <icon class="icon-box-img" type="download"></icon>
      <icon class="icon-box-img" type="search"></icon>
      <icon class="icon-box-img" type="clear"></icon>
    </view>

    <!--   not working: not display correctly   -->
    <view class="progress-box">
      <progress :percent="20" show-info :stroke-width="3" />
    </view>

    <view class="progress-box">
      <progress :percent="40" active :stroke-width="3" />
      <icon class="progress-cancel" type="cancel"></icon>
    </view>

    <view class="progress-box">
      <progress :percent="60" active :stroke-width="3" />
    </view>

    <view class="progress-box">
      <progress :percent="80" :color="'#10AEFF'" active :stroke-width="3" />
    </view>

    <rich-text :nodes="htmlSnip"></rich-text>
    <rich-text :nodes="nodes"></rich-text>
  </view>
</template>
<script>
import Taro from "@tarojs/taro";
import styles from "./home.module.less";

export default {
  data() {
    return {
      styles,
      background: ["demo-text-1", "demo-text-2", "demo-text-3"],
      msg: "Hello world!",
      htmlSnip: `<div class="div_class">
  <h1>Title</h1>
  <p class="${styles["styled-p"]}">
    Life is&nbsp;<i>like</i>&nbsp;a box of
    <b>&nbsp;chocolates</b>.
  </p>
</div>
`,
      nodes: [
        {
          name: "div",
          attrs: {
            class: "div_class",
            style: "line-height: 60px; color: #1AAD19;"
          },
          children: [
            {
              type: "text",
              text: "You never know what you're gonna get."
            }
          ]
        }
      ],
      checkItems: [
        { value: "USA", name: "美国" },
        { value: "CHN", name: "中国", checked: "true" },
        { value: "BRA", name: "巴西" },
        { value: "JPN", name: "日本" },
        { value: "ENG", name: "英国" },
        { value: "FRA", name: "法国" }
      ]
    };
  },
  mounted() {},
  methods: {
    gotoInfo() {
      Taro.navigateTo({ url: "/pages/info/info" });
    },
    editorReady() {
      const query = Taro.createSelectorQuery();
      const editor = query.select("#editor");
      console.log(editor);
      editor.context(res => {
        console.log("$$$$$$$$$$$$$$$$$$$$$$$$$$$$$", res.context);
        res.context.setContents({ html: this.htmlSnip });
      });
    }
  }
};
</script>
