<!--    vue页面：Messages.vue     -->
<!--
    组件：网站的消息页面
    功能：显示用户的消息
    更新日志：
-->
<i18n>
{
  "CHS": {
    "MsgCenter":"消息中心",
    "UnreadMsg":"未读消息",
    "AllMsg":"全部消息"
  },
  "ENG": {
    "MsgCenter":"Message Center",
    "UnreadMsg":"Unread Messages",
    "AllMsg":"All Messages"
  },
  "CHT": {
    "MsgCenter":"消息中心",
    "UnreadMsg":"未讀消息",
    "AllMsg":"全部消息"
  }
}
</i18n>

<template>
  <div class="messages">
    <topnavbar />
    <div class="main">
      <!-- 左面的导航条 -->
      <div class="messageNav">
        <div class="messageNav-title">
          <i class="el-icon-s-promotion"></i>
          {{ $t("MsgCenter") }}
        </div>
        <ul class="messageNav-list">
          <li v-bind:class="{ messageNavListActive: messageType == 0 }" @click="messageType=0">
            <i class="el-icon-star-on"></i>
            {{ $t("UnreadMsg") }}
          </li>
          <li v-bind:class="{ messageNavListActive: messageType == 1 }" @click="messageType=1">
            <i class="el-icon-star-on"></i>
            {{ $t("AllMsg") }}
          </li>
        </ul>
      </div>
      <!-- 右面的详情 -->
      <div class="msgDetail">
        <unreadMsg v-if="messageType == 0"></unreadMsg>
        <allMsg v-if="messageType == 1"></allMsg>
      </div>
    </div>
  </div>
</template>

<script>
import topnavbar from "../components/TopNavbar.vue";
import unreadMsg from "../components/messageCompoents/UnreadMsg.vue";
import allMsg from "../components/messageCompoents/AllMessage.vue";
export default {
  data() {
    this.$i18n.locale = localStorage.getItem("lang");
    return {
      // 显示信息类型，0 代表未读消息，1 代表已读消息
      messageType: 0
    };
  },
  created() {
    // 初始化页面名为 home
    this.$store.commit("changeBgc", "home");
    // 修改网站标题
    document.title = "消息中心 - Patchyvideo";
  },
  methods: {},
  components: { topnavbar, unreadMsg, allMsg }
};
</script>

<style scoped>
.messages {
  background: url("../static/img/messages.jpg") no-repeat;
  background-size: cover;
  background-position: 0px 50px;
  height: 100%;
}
.main {
  width: 80%;
  height: calc(100% - 80px);
  margin: 0 auto;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
  background-color: rgba(255, 255, 255, 0.466);
  display: flex;
}
.messageNav {
  width: 180px;
  height: 100%;
  background-color: rgba(255, 255, 255, 0.767);
}
.messageNav-title {
  padding: 15px;
  font-size: 20px;
}
.messageNav-list li {
  padding: 8px;
}
.messageNavListActive,
.messageNav-list li:hover {
  color: #409eff;
  cursor: pointer;
}
.msgDetail {
  width: calc(100% - 180px);
  height: 100%;
  background-color: rgba(0, 136, 255, 0.123);
}
</style>
