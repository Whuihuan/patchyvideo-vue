<!--    vue页面：ListDetail.vue     -->
<!--
    页面：视频列表的详细信息
    功能：展示展示视频列表的详细信息
    包含组件：LeftNavbar.vue、TopNavbar.vue、Foot.vue、EditTags
    更新日志：
    12/29/2019: v1.0 
      release
    1/9/2020：v1.0.1
      1.加入了Tag编辑功能
    1/16/2020:v1.0.2
      1.增加EditTags对Postvideo的支持。
      2.增加Move、DeleteVideo、SetCover 操作页面的组件，样式微调。
    1/19/2020：v1.0.2
      1.新增字体图标以及对应的元素操作。
      （由于是百分百布局，存在滚动条问题，Dialog对话框触发后滚动条消失，页面会突然向右移动铺满）
    1/20/2020：v1.0.3
      1.新增添加视频 编辑 删除模块，但是现阶段接口不支持。
    2/1/2020：v1.0.4
      1.添加视频 编辑 删除模块完成
      2.页面上的编辑标签在列表不可编辑或者未登录的时候隐藏
    2/2/2020：v1.0.5
      1.视频列表的索引值优化
    2/4/2020：v1.0.6
      1.添加视频的时候视频添加到播放列表的末尾而不是开头
    2/7/2020：v1.0.7
      1.tag编辑标签之前会请求标签数据
    ★待解决问题：
      1.播放列表里链接的复制功能因为涉及到对dom的直接操作，所以可能会有被抓住漏洞的风险
      2.编辑共有标签的时候有时候会弹出“添加成功”的提示
-->

<i18n>
{
  "CHS": {
    "btn_group":{
        "add_video":"添加视频",
        "import_from_other":"从其他网站的收藏夹导入",
        "add_favorite":"加入收藏",
        "edit_list_info":"编辑列表信息",
        "edit_common_tags":"编辑共有标签",
        "reverse_list":"列表视频倒序",
        "delete":"删除"
    },
    "vedio_chain_tip":"在此插入视频",
    "edit_list_info_dialog":{
        "title":"编辑视频详情",
        "list_title_tip":"这里是列表标题",
        "list_title_err_tip":"还没输入标题呢",
        "list_introduction_tip":"来介绍一下自己的列表吧",
        "list_introduction_err_tip":"不来介绍一下列表吗？",
        "set_private_list":"设为私有列表",
        "btn_ok":"确认修改",
        "btn_cancel":"取 消"
    },
    "delete_dialog":{
        "title":"提示",
        "content":"确认删除吗？",
        "btn_ok":"确定",
        "btn_cancel":"取 消"
    },
    "commit_tip":"提交成功",
    "oper_tip":"操作成功",
    "delete_tip":"删除成功"
  },
  "ENG": {
    "btn_group":{
        "add_video":"Add video",
        "import_from_other":"Extend from playlist of other websites",
        "add_favorite":"Add to folder",
        "edit_list_info":"Edit playlist information",
        "edit_common_tags":"Edit common tags",
        "reverse_list":"Reverse video order",
        "delete":"Delete"
    },
    "vedio_chain_tip":"Insert video here",
    "edit_list_info_dialog":{
        "title":"Edit video details",
        "list_title_tip":"Playlist title",
        "list_title_err_tip":"Please enter playlist title",
        "list_introduction_tip":"Playlist description",
        "list_introduction_err_tip":"Please enter playlist description",
        "set_private_list":"Private playlist",
        "btn_ok":"Submit",
        "btn_cancel":"Cancel"
    },
    "delete_dialog":{
        "title":"Confirm",
        "content":"Are you sure you want to delete this playlist? This operation is irreversible.",
        "btn_ok":"Yes",
        "btn_cancel":"Cancel"
    },
    "commit_tip":"Submit succeed",
    "oper_tip":"Operation succeed",
    "delete_tip":"Delete succeed"
  },
  "CHT": {
    "btn_group":{
        "add_video":"添加視頻",
        "import_from_other":"從其他網站的收藏夾導入",
        "add_favorite":"加入收藏",
        "edit_list_info":"編輯列表信息",
        "edit_common_tags":"編輯共有標簽",
        "reverse_list":"列表視頻倒序",
        "delete":"刪除"
    },
    "vedio_chain_tip":"在此插入視頻",
    "edit_list_info_dialog":{
        "title":"編輯視頻詳情",
        "list_title_tip":"這裏是列表標題",
        "list_title_err_tip":"還沒輸入標題呢",
        "list_introduction_tip":"來介紹壹下自己的列表吧",
        "list_introduction_err_tip":"不來介紹壹下列表嗎？",
        "set_private_list":"設為私有列表",
        "btn_ok":"確認修改",
        "btn_cancel":"取 消"
    },
    "delete_dialog":{
        "title":"提示",
        "content":"確認刪除嗎？",
        "btn_ok":"確定",
        "btn_cancel":"取 消"
    },
    "commit_tip":"提交成功",
    "oper_tip":"操作成功",
    "delete_tip":"刪除成功"
  }
}
</i18n>


<template>
  <div class="listDetail">
    <topnavbar />

    <!-- EditTags 组件-->
    <EditTags
      ref="editTag"
      :msg="temporaryValForVLP"
      :visible.sync="showTagPanel"
      v-if="showTagPanel"
    ></EditTags>

    <!-- 编辑视频列表时的对话框 -->
    <el-dialog
      :title="$t('edit_list_info_dialog.title')"
      :visible.sync="openListEdit"
      width="40%"
      :close-on-click-modal="false"
    >
      <el-form ref="list" :model="playlist_metadata" label-width="auto" :rules="rules">
        <!-- 标题 -->
        <el-form-item prop="title">
          <el-input
            v-model="playlist_metadata.title"
            :placeholder="$t('edit_list_info_dialog.list_title_tip')"
          ></el-input>
        </el-form-item>
        <!-- 简介 -->
        <el-form-item prop="desc">
          <el-input
            type="textarea"
            :autosize="{ minRows: 6 }"
            :placeholder="$t('edit_list_info_dialog.list_introduction_tip')"
            v-model="playlist_metadata.desc"
          ></el-input>
        </el-form-item>
        <el-form-item>
          <el-checkbox
            v-model="playlist_metadata.private"
          >{{$t('edit_list_info_dialog.set_private_list')}}</el-checkbox>
        </el-form-item>
        <el-form-item class="createList">
          <el-button
            type="primary"
            @click="onSubmit"
            style="width:80%"
            :loading="loading"
          >{{$t('edit_list_info_dialog.btn_ok')}}</el-button>
          <el-button
            @click="openListEdit = false"
            style="width:80%;margin-top:10px;margin-left:0px"
          >{{$t('edit_list_info_dialog.btn_cancel')}}</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!-- 删除列表时的确认框 -->
    <el-dialog :title="$t('delete_dialog.title')" :visible.sync="dialogVisible" width="30%">
      <span>{{$t('delete_dialog.content')}}</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">{{$t('delete_dialog.btn_cancel')}}</el-button>
        <el-button
          type="primary"
          @click="dialogVisible = false; deleteVideoList();"
        >{{$t('delete_dialog.btn_ok')}}</el-button>
      </span>
    </el-dialog>

    <!-- listdetail页面的正文 -->
    <div class="w main-page-background-img" v-loading="loading">
      <div class="content">
        <!-- 视频列表介绍 -->
        <div class="deemo shadow">
          <div class="d_t">
            <!--<img src="../static/img/5.png" style="float:left;margin-top:50px;" />
            <img src="../static/img/1.png" style="float:right;margin-top:50px;" />-->
            <h2>{{ videolistName }}</h2>
            <img :src="'/images/covers/' + videolistDetail.playlist.cover" style="min-height:200px" />
            <p>{{ videolistDesc }}</p>
          </div>
          <!-- 打开 Tag 编辑页面 -->
          <div v-if="editable" class="edit_box">
            <el-button type="success" @click="addVideo">{{$t('btn_group.add_video')}}</el-button>
            <el-button type="success" @click="addFromList">{{$t('btn_group.import_from_other')}}</el-button>

            <!-- 注意！此处的样式修改在 App.vue 的全局样式中 -->
            <el-popover style="margin: 0px 10px;" width="100%" trigger="click">
              <ListFolderView
                ref="listFolder"
                :msg="temporaryValForVLP"
                :visible.sync="showListFolder"
                v-if="isLogin"
              ></ListFolderView>
              <el-button
                type="primary"
                @click="openListFolder"
                class="EditTagsButton"
                slot="reference"
              >{{$t('btn_group.add_favorite')}}</el-button>
            </el-popover>

            <el-button type="info" @click="openListEdit = true">{{$t('btn_group.edit_list_info')}}</el-button>

            <el-button
              type="primary"
              @click="openEditTags"
              class="EditTagsButton"
              :disabled="showTagPanel"
            >{{$t('btn_group.edit_common_tags')}}</el-button>

            <el-button type="warning" @click="inverse()">{{$t('btn_group.reverse_list')}}</el-button>
            <el-button type="danger" @click="dialogVisible = true">{{$t('btn_group.delete')}}</el-button>
          </div>
          <!-- 没有编辑权限的时候只提供加入收藏功能 -->
          <div v-else>
            <el-popover v-if="isLogin" style="margin: 0px 10px;" width="100%" trigger="click">
              <ListFolderView
                ref="listFolder"
                :msg="temporaryValForVLP"
                :visible.sync="showListFolder"
                v-if="isLogin"
              ></ListFolderView>
              <el-button
                type="primary"
                @click="openListFolder"
                class="EditTagsButton"
                slot="reference"
              >{{$t('btn_group.add_favorite')}}</el-button>
            </el-popover>
          </div>
          <!-- 评分区 -->
          <div>
            <Score type="playlist"></Score>
          </div>
        </div>

        <!-- 视频列表 -->
        <div class="recommend">
          <!-- 视频详情 -->
          <div class="minbox shadow" v-for="(item, index) in videolistVideos" :key="item._id.$oid">
            <div class="re_video">
              <div class="edit">
                <div id="edit_first">
                  <h1>{{ item.rank + 1 }}</h1>
                </div>
                <div v-if="editable" id="edit_second">
                  <Move class="move" :msg="PlaylistItemOp(item, index)"></Move>
                  <!--上移-->
                </div>
              </div>

              <img class="re_video_img" :src="'/images/covers/' + item.item.cover_image" />
              <div class="re_video_desc">
                <el-tooltip
                  v-if="editable"
                  class="item"
                  effect="dark"
                  :content="$t('btn_group.add_video')"
                  placement="top"
                >
                  <router-link
                    :to="{
                      path: './postvideo',
                      query: getInsertData(item, index)
                    }"
                    class="insert-video"
                  >
                    <i class="fa fa fa-plus" aria-hidden="true"></i>
                  </router-link>
                </el-tooltip>

                <h3>
                  <router-link
                    target="_blank"
                    :to="{ path: '/video', query: { id: item._id.$oid } }"
                    tag="a"
                  >{{ item.item.title }}</router-link>
                </h3>
                <p>{{ item.item.desc }}</p>
                <div>
                  <img
                    :src="require('../static/img/' + item.item.site + '.png')"
                    width="16px"
                    style="margin-right:2px"
                  />
                  <a :href="item.item.url">
                    {{
                    item.item.url
                    }}
                  </a>
                  <i
                    @click="copyVideoLink(item.item.url)"
                    class="fa fa-copy fa-lg"
                    style="margin-left:2px"
                  ></i>
                </div>
              </div>
              <div v-if="editable" class="item_end">
                <SetCover class="set-cover" :msg="PlaylistItemOp(item, index)"></SetCover>
                <DeleteVideo class="delete-video" :msg="PlaylistItemOp(item, index)"></DeleteVideo>
              </div>
            </div>
          </div>
        </div>

        <!-- ElementUI 自带的分页器 -->
        <el-pagination
          background
          class="page-selector"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          layout="jumper, prev, pager, next, sizes"
          :current-page="this.page"
          :total="this.maxcount"
          :page-size="20"
          :page-sizes="[10, 20, 30, 40]"
        ></el-pagination>

        <!-- 评论区 -->
        <div>
          <Comments :sid="sid"></Comments>
        </div>
      </div>
    </div>

    <Footer></Footer>
  </div>
</template>

<script>
import topnavbar from "../components/TopNavbar.vue";
import Footer from "../components/Footer.vue";
import EditTags from "../components/EditTags.vue";
import Move from "../components/Move.vue";
import DeleteVideo from "../components/DeleteVideo.vue";
import SetCover from "../components/SetCover.vue";
import ListFolderView from "../components/ListFolderView.vue";
import Comments from "../components/comments.vue";
import Score from "../components/Score.vue";
import { copyToClipboardText } from "../static/js/generic";

export default {
  data() {
    this.$i18n.locale = localStorage.getItem("lang");
    return {
      // 视频列表的元信息
      playlist_metadata: {
        title: "",
        desc: "",
        cover: "",
        private: false
      },
      // 视频列表的详细信息
      videolistDetail: {
        playlist: {
          cover: ""
        }
      },
      // 判断是否登录
      isLogin: false,
      // 视频编辑功能是否可见的标志
      editable: false,
      // 视频列表的名称
      videolistName: "",
      // 视频列表的介绍
      videolistDesc: "",
      // 视频列表里的视频
      videolistVideos: [],
      // 当前页数
      page: 1,
      // 全部分页数
      maxpage: 1,
      // 每一页的视频数量
      count: 20,
      // 视频的全部数量
      maxcount: 0,
      // 列表评论的 sid
      sid: "",
      // 传入 Tags 组件视频页的 ID
      videolistPid: "",
      //传入 Tags 组件视频页的 ID 临时变量
      temporaryValForVLP: "",
      // 视频列表是否属于加载状态的判断
      loading: true,
      // 打开列表详情编辑页面
      openListEdit: false,
      // 打开列表标签编辑页面
      showTagPanel: false,
      // 打开删除列表的确认界面
      dialogVisible: false,
      // 编辑列表详情的校验数据
      rules: {
        title: [
          {
            required: true,
            message: this.$t("edit_list_info_dialog.list_title_err_tip"),
            trigger: "blur"
          }
        ],
        desc: [
          {
            required: true,
            message: this.$t("edit_list_info_dialog.list_introduction_err_tip"),
            trigger: "blur"
          }
        ]
      },
      // 播放列表目录页面是否显示
      showListFolder: false
    };
  },
  computed: {},
  mounted() {
    // 查看是否登录
    if (
      JSON.stringify(this.$store.state.username) != "null" &&
      this.$store.state.username != ""
    ) {
      this.isLogin = true;
    }
    this.getVideoList(this.page, this.count);
  },
  methods: {
    // 获取插入视频需要的数据
    getInsertData(e, i) {
      let obj = {
        pid: this.videolistPid,
        rank: e.rank
      };
      return obj;
    },
    // 获取移动组件所需要的数据
    PlaylistItemOp(e, i) {
      let obj = {
        //移动组件所需要的数据
        pid: this.videolistPid,
        vid: e._id.$oid,
        page: this.page,
        page_size: this.maxcount
      };

      return obj;
    },
    // 当前播放列表的页面切换的时候调用
    handleCurrentChange(val) {
      this.page = val;
    },
    // 当前页面显示视频条数切换的时候调用
    handleSizeChange(val) {
      this.count = val;
    },
    // 请求单个播放列表详细数据
    getVideoList: function(e, count) {
      // 先使页面出于加载状态
      this.loading = true;

      this.axios({
        method: "post",
        url: "be/lists/get_playlist.do",
        data: { page: e, page_size: count, pid: this.$route.query.id }
      })
        .then(result => {
          // 请求失败的情况
          if (result.data.status == "FAILED") {
            // 列表不存在的情况，跳转到 404 页面
            if (result.data.data.reason == "PLAYLIST_NOT_EXIST") {
              this.$router.push({ path: "*" });
            }
          }
          // 页面存在的情况下
          this.videolistDetail = result.data.data;
          // 必须是登录且发来的数据是可编辑的才渲染编辑组件
          this.editable = this.videolistDetail.editable && this.isLogin;
          this.videolistName = this.videolistDetail.playlist.title.english;
          // 修改网站标题
          document.title = this.videolistName;
          this.videolistDesc = this.videolistDetail.playlist.desc.english;
          this.videolistVideos = this.videolistDetail.videos;
          this.videolistPid = this.videolistDetail.playlist._id.$oid;
          this.maxcount = result.data.data.count;
          this.maxpage = result.data.data.page_count;
          if (result.data.data.playlist.comment_thread) {
            this.sid = result.data.data.playlist.comment_thread.$oid;
          }

          // 请求单个播放列表的元数据
          this.axios({
            method: "post",
            url: "be/lists/get_playlist_metadata.do",
            data: {
              pid: this.videolistPid
            }
          }).then(result => {
            this.playlist_metadata.title =
              result.data.data.playlist.title.english;
            this.playlist_metadata.desc =
              result.data.data.playlist.desc.english;
            this.playlist_metadata.private = result.data.data.playlist.private;
          });

          // 加载结束，加载动画消失
          this.loading = false;

          // 回到顶部
          if ($("html").scrollTop()) {
            // 动画效果
            $("html").animate({ scrollTop: 0 }, 100);
          }
        })
        .catch(error => {
          this.$router.push({ path: "/404" });
        });
    },
    // 复制视频连接
    copyVideoLink: function(url) {
      this.$alert(
        "视频链接复制" + (copyToClipboardText(url) ? "成功！" : "失败！"),
        "分享链接",
        {
          confirmButtonText: "确定"
        }
      );
    },
    // 添加视频
    addVideo() {
      this.$router.push({
        path: "./postvideo",
        query: {
          pid: this.videolistPid,
          rank: -1
        }
      });
    },
    // 从其他网站的收藏夹导入视频
    addFromList() {
      this.$router.push({
        path: "./createVideoList",
        query: {
          pid: this.videolistPid,
          exist: 1
        }
      });
    },
    // 提交视频详情修改数据
    onSubmit() {
      this.loading = true;
      this.axios({
        method: "post",
        url: "be/lists/update_playlist_metadata.do",
        data: {
          pid: this.videolistPid,
          title: this.playlist_metadata.title,
          desc: this.playlist_metadata.desc,
          private: this.playlist_metadata.private
        }
      }).then(res => {
        this.open(this.$t("commit_tip"));
        this.openListEdit = false;
        this.getVideoList();
      });
    },
    // 列表视频倒序显示
    inverse() {
      this.loading = true;
      this.axios({
        method: "post",
        url: "/be/list/inverse.do",
        data: {
          pid: this.videolistPid
        }
      }).then(res => {
        this.open(this.$t("oper_tip"));
        this.getVideoList();
      });
    },
    //删除列表
    deleteVideoList: function() {
      this.axios({
        method: "post",
        url: "be/lists/del_playlist.do",
        data: {
          pid: this.videolistPid
        }
      }).then(res => {
        this.open(this.$t("delete_tip"));
        this.$router.push({ path: "/lists" });
      });
    },
    // 打开 Tag 编辑页面
    openEditTags: function() {
      this.temporaryValForVLP = this.videolistPid;
      this.showTagPanel = true;
      // this.$refs.editTag.getCommonTags();
    },
    openListFolder() {
      this.showListFolder = true;
      this.$refs.listFolder.pid = this.videolistPid;
    },
    open(message) {
      this.$message({
        message: message,
        type: "success"
      });
    }
  },
  computed: {
    f1() {
      return this.$store.state.refreshCount;
    }
  },
  watch: {
    page(v) {
      this.getVideoList(this.page, this.count);
    },
    count(v) {
      this.getVideoList(this.page, this.count);
    },
    f1() {
      this.getVideoList(this.page, this.count);
    }
  },
  components: {
    topnavbar,
    Footer,
    EditTags,
    Move,
    DeleteVideo,
    ListFolderView,
    SetCover,
    Comments,
    Score
  }
};
</script>

<style scoped lang="less">
.insert-video {
  position: absolute;
  left: 35%;
  transform: translateY(-50px);
  i {
    width: 25px;
    height: 25px;
    border-radius: 50%;
    color: white;
    border: 3px solid #006d88;
    text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 20px #228dff, 0 0 40px #228dff;
    text-align: center;
    background-color: black;

    z-index: 1;
    font-size: 25px;
    transition: all 0.3s ease;
  }
}
.re_video {
  display: flex;
  /* justify-content: center; */
  align-items: center;
  .edit {
    width: 150px;
    display: flex;
    #edit_first {
      // h1
      font-size: 30px;
      min-width: 90px;
      color: rgb(98, 169, 231);
      flex-grow: 1;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    #edit_first h1 {
      flex-shrink: 0;
    }
    #edit_second {
      // icon
      /deep/ .move-down-box {
        transform: translateX(-10px);
      }

      /deep/ .move {
        button {
          &:hover {
            background: none;
          }
        }
        display: flex;
        height: 100%;
        flex-direction: column;

        font: normal normal normal 14px/1 FontAwesome;
        font-size: inherit;
        text-rendering: auto;
        -webkit-font-smoothing: antialiased;

        /deep/ .move-up {
          font-size: 40px;
          height: 40px;
          /* margin-bottom: 50px; */
          color: #808080;
          transition: all 0.4s ease;
          // position: absolute;
          // top: -20px;
          &:hover {
            color: white;
            text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 20px #228dff,
              0 0 40px #228dff;
          }
          /deep/ .move-down {
            font-size: 40px;
            height: 40px;
            color: #808080;
            transition: all 0.4s ease;

            // position: absolute;
            // bottom: 0px;
          }
        }
        /deep/ .move-down {
          font-size: 40px;
          height: 40px;
          color: #808080;
          transition: all 0.4s ease;

          &:hover {
            color: white;
            text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 20px #228dff,
              0 0 40px #228dff;
          }

          // position: absolute;
          // bottom: 0px;
        }
      }
    }
  }
}
.item_end {
  width: 10%;
  display: flex;
  justify-content: center;

  .delete-video {
    flex: 1;
    height: 30px;
    font-size: 30px;
    text-align: center;
    /deep/ i {
      &:hover {
        transition: all 0.4s ease;
        color: white;
        text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 20px #228dff,
          0 0 40px #228dff;
      }
    }
  }
  .set-cover {
    text-align: center;
    font-size: 30px;
    height: 30px;
    flex: 1;
    &:hover {
      transition: all 0.4s ease;
      color: white;
      text-shadow: 0 0 5px #fff, 0 0 10px #fff, 0 0 20px #228dff,
        0 0 40px #228dff;
    }
  }
}

.shadow {
}
.content {
  top: 3px;
  width: 80%;
  position: relative;
  flex: 1;
}
.main-page-background-img {
  background-repeat: no-repeat;
  min-height: 800px;
  width: 85%;
  margin-top: 20px;
}
.d_t {
  width: 100%;
  margin-bottom: 0px;
  padding-bottom: 5px;
}
.d_t h2 {
  padding-top: 20px;
}
.d_t p {
  width: 60%;
  text-align: center;
  white-space: pre-line;
  margin: 0px auto;
}
.d_t img {
  height: 200px;
  margin: 10px;
  background-color: rgba(255, 255, 255, 0);
}

.EditTagsButton {
  /* width: 70%; */
  margin-bottom: 20px;
}

.minbox {
  width: 1200px;
  margin-left: 12.5px;
  margin-right: 12.5px;
  margin-top: 30px;
  padding: 20px 0px;
}

.re_top {
  width: calc(100% - 20px);
  margin: 0 auto;
  margin-top: 20px;
  padding-bottom: 20px;
  border-bottom: 3px solid red;
}
.re_top h5 {
  margin-right: 5px;
}
.re_video {
  text-align: left;
  /* height: 150px; */
}
.re_video h1 {
}
.re_video_img {
  display: inline-block;
  width: 200px;
  height: 125px;
  margin-right: 20px;
  min-width: 200px;
  min-height: 125px;
}
.re_video_desc {
  width: 679px;
  display: inline-block;
  vertical-align: top;
  white-space: pre-wrap;
  position: relative;
}
.re_video_desc p {
  font-size: 1rem;
  line-height: 1.1rem;
  height: 4.3rem;
  white-space: pre-wrap;
  margin-top: 10px;
  margin-bottom: 10px;
  /* 使文字变为最多显示 4 行，多余的使用省略号代替 */
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
}

.page-selector {
  display: block;
  text-align: center;
  margin-top: 20px;
}

.fa-copy:hover {
  color: olive;
  cursor: pointer;
}

.createList {
  text-align: center;
}
</style>
