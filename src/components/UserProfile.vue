<!--
    页面：paychyvideo的个人信息界面
    功能：展示用户个人信息
    更新日志：
    12/31/2019: v1.0

    ★待解决问题：
     因个人界面接口较特殊，需登录后才能获取，登录需携带Cookie，请先在官网登录后再打开此页面。
     本地登录提示错误暂未做处理。

       更新日志：
       1/11/2020
       问题：
       1.登录机制需要做成cookie保存验证判断,现有的登录有点问题暂时没有解决，
       2.由于User组件被改动，与原先的单独渲染相反，现在的所有子组件渲染几乎同时完成，意味着只要用户进入user界面，其他三个页面就已经加载完成。
       3.该组件应当采用大驼峰命名，下次更新时修改。
       2/11/2020：v1.0.0
       1.新增图片裁剪组件
       需要安装依赖：npm install vue-cropper -s


-->

<i18n>
{
  "CHS": {
    "cur_pic": "当前头像",
    "pic_prompt": "选择图片上传：大小256 * 256像素",
    "change_username": "更改用户名",
    "update": "更改",
    "cancel": "取消",
    "save": "保存",
    "change_pass": "更改密码",
    "old_pass": "旧密码",
    "new_pass": "新密码",
    "confirm_pass": "确认密码",
    "submit": "提交",
    "reset": "重置",
    "bind_mail": "绑定邮箱",
    "enter_email": "请输入邮箱",
    "bind": "绑定",
    "enter_old_pass": "请输入旧密码",
    "pass_short": "密码长度必须至少为6个字符",
    "pass_long": "密码长度必须至多为64个字符",
    "enter_new_pass": "请输入新密码",
    "enter_new_pass_again": "请再次输入新密码",
    "pass_mismatch": "两次输入密码不一致!",
    "invalid_email": "请检查所填写邮箱是否合法",
    "bind_succeed": "绑定成功",
    "check_form": "请检查所填写信息是否正确",
    "update_succeed": "修改成功！",
    "update_failed": "更改失败！",
    "desc_long": "字数不能超过2000哦！",
    "user_exist": "用户名已存在！",
    "user_length": "用户名长度应在 2 到 32 个字符！",
    "wrong_pass": "请检查旧密码是否正确"
  },
  "ENG": {
    "cur_pic": "Current photo",
    "pic_prompt": "Upload photo, it will be scaled to 256x256",
    "change_username": "Update username",
    "update": "Update",
    "cancel": "Cancel",
    "save": "Save",
    "change_pass": "Update password",
    "old_pass": "Current password",
    "new_pass": "New password",
    "confirm_pass": "Confirm password",
    "submit": "Submit",
    "reset": "Reset",
    "bind_mail": "Email",
    "enter_email": "Enter email",
    "bind": "Confirm",
    "enter_old_pass": "Please enter current password",
    "pass_short": "Password must be at least 6 characters long",
    "pass_long": "Password length exceed 64 characters",
    "enter_new_pass": "Please enter new password",
    "enter_new_pass_again": "Please enter confirm password",
    "pass_mismatch": "Passwords mismatch",
    "invalid_email": "Invalid email address",
    "bind_succeed": "Email update succeed",
    "check_form": "Please check if you have filled all information correctly",
    "update_succeed": "Update succeed",
    "update_failed": "Update failed",
    "desc_long": "Description too long (max 2000 characters)",
    "user_exist": "This username has been taken",
    "user_length": "Username length must be between 2 to 32 characters",
    "wrong_pass": "Incorrect password"
  },
  "CHT": {
    "cur_pic": "當前頭像",
    "pic_prompt": "選擇圖片上傳：大小256 * 256像素",
    "change_username": "更改用戶名",
    "update": "更改",
    "cancel": "取消",
    "save": "保存",
    "change_pass": "更改密碼",
    "old_pass": "舊密碼",
    "new_pass": "新密碼",
    "confirm_pass": "確認密碼",
    "submit": "提交",
    "reset": "重置",
    "bind_mail": "綁定郵箱",
    "enter_email": "請輸入郵箱",
    "bind": "綁定",
    "enter_old_pass": "請輸入舊密碼",
    "pass_short": "密碼長度必須至少為6個字符",
    "pass_long": "密碼長度必須至多為64個字符",
    "enter_new_pass": "請輸入新密碼",
    "enter_new_pass_again": "請再次輸入新密碼",
    "pass_mismatch": "兩次輸入密碼不壹致!",
    "invalid_email": "請檢查所填寫郵箱是否合法",
    "bind_succeed": "綁定成功",
    "check_form": "請檢查所填寫信息是否正確",
    "update_succeed": "修改成功！",
    "update_failed": "更改失敗！",
    "desc_long": "字數不能超過2000哦！",
    "user_exist": "用戶名已存在！",
    "user_length": "用戶名長度應在 2 到 32 個字符！",
    "wrong_pass": "請檢查舊密碼是否正確"
  }
}
</i18n>

<template>
  <div>
    <!--<el-button @click="testIpfs">dwa</el-button>-->
    <div class="bigbox standard" v-if="this.$route.params.id=='me'" v-loading="loading">
      <div class="bigbox_left">
        <div class="left-content">
          <div class="wave ripple danger">
            <div class="circle" :class="{animeActive1:mounseMark}"></div>
            <div class="circle" :class="{animeActive2:mounseMark}"></div>
            <div class="circle" :class="{animeActive3:mounseMark}"></div>
          </div>
          <div class="face" @mouseover="faceMouseOver(true)" @mouseleave="faceMouseOver(false)">
            <img :src="this.url" alt v-if="this.url!=''" />
            <img :src="'be/images/userphotos/'+myData.image" alt v-if="this.url===''" />
          </div>

          <p>{{$t('cur_pic')}}</p>
          <p>{{$t('pic_prompt')}}</p>
          <div>
            <App-cropper
              :width="300"
              :height="300"
              :fixed-number="[1,1]"
              @subUploadSucceed="getShopImages"
            />
          </div>
          <!--<form
            action="/be/helper/upload_image.do"
            method="post"
            accept-charset="utf-8"
            enctype="multipart/form-data"
            id="form1"
            @submit.prevent="sub"
          >
            <input id="type" name="type" type="text" value="userphoto" v-show="false" />
            <input id="file" name="file" type="file" tag="input" />
            <el-input type="submit" value="上传"></el-input>
          </form>-->
        </div>
      </div>

      <div class="bigbox_left" id="imoto2"></div>
      <div class="bigbox_right">
        <div class="desc">
          <div class="desc_name" style="display: flex;height:30px; ">
            <p v-if="isNameEdit===false" style="margin-right: 10px">{{myData.username}}</p>
            <i v-if="isNameEdit===false" class="el-icon-edit" @click="islSetUserName(true)"></i>
            <el-input
              v-if="isNameEdit===true"
              :placeholder="$t('change_username')"
              prefix-icon="el-icon-user"
              v-model="myName"
            ></el-input>
            <el-button
              v-if="isNameEdit===true"
              type="primary"
              icon="el-icon-edit"
              :disabled="myName===''"
              @click="setUserName"
            >{{$t('update')}}</el-button>
            <el-button
              v-if="isNameEdit===true"
              type="primary"
              @click.native="islSetUserName(false)"
            >{{$t('cancel')}}</el-button>
          </div>

          <div class="text-form">
            <textarea name v-model="myData.desc" cols="30" rows="10"></textarea>
          </div>
          <button @click="changeDesc()">{{$t('save')}}</button>
        </div>
        <div class="psd">
          <div class="desc_name">{{$t('change_pass')}}</div>
          <div class="psd-form">
            <!--<input type="password" placeholder="Old Password">
            <input type="password" placeholder="New Password">
            <input type="password" placeholder="Repeat New Password">-->
            <el-form
              :model="ruleForm"
              status-icon
              :rules="rules"
              ref="ruleForm"
              label-width="100px"
              class="demo-ruleForm"
            >
              <el-form-item :label="$t('old_pass')" prop="old_pass">
                <el-input v-model.number="ruleForm.old_pass"></el-input>
              </el-form-item>
              <el-form-item :label="$t('new_pass')" prop="pass">
                <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item :label="$t('confirm_pass')" prop="checkPass">
                <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item class="post">
                <el-button type="primary" @click="submitForm('ruleForm')">{{$t('submit')}}</el-button>
                <el-button @click="resetForm('ruleForm')">{{$t('reset')}}</el-button>
              </el-form-item>
            </el-form>
          </div>

          <!-- <button>保存</button> -->
        </div>
        <div class="email">
          <div class="email-info">{{$t('bind_mail')}}</div>

          <el-input
            :placeholder="$t('enter_email')"
            prefix-icon="el-icon-message"
            v-model="myEmail"
          ></el-input>
          <button @click="bindEmail()">{{$t('bind')}}</button>
          <p style="margin-top: 20px" v-if="myData.email!==''">已绑定邮箱:{{myData.email}}</p>
        </div>
      </div>
    </div>
    <div class="bigbox standard" v-if="this.$route.params.id!='me'" v-loading="loading">
      <div class="bigbox_left" :class="{bg:this.$route.params.id!='me'}"></div>
      <div class="bigbox_right">
        <div class="face2">
          <div class="pulse"></div>
          <img :src="'be/images/userphotos/'+userData.image" alt />
        </div>
        <div class="desc">
          <div class="desc_name">{{userData.username}}</div>
          <div class="text-form">
            <textarea name id cols="30" rows="10" disabled="disabled">{{userData.desc}}</textarea>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppCropper from "@/components/Cropper";
export default {
  data() {
    this.$i18n.locale = localStorage.getItem("lang");
    var validateOldPass = (rule, value, callback) => {
      if (!value) {
        return callback(new Error(this.$t("enter_old_pass")));
      }
      if (JSON.stringify(value).length < 8) {
        return callback(new Error(this.$t("pass_short")));
      }
      if (JSON.stringify(value).length > 64) {
        return callback(new Error(this.$t("pass_long")));
      }
      callback();
    };
    var validatePass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error(this.$t("enter_new_pass")));
      } else {
        if (JSON.stringify(value).length < 10) {
          return callback(new Error(this.$t("pass_short")));
        }
        if (JSON.stringify(value).length > 66) {
          return callback(new Error(this.$t("pass_long")));
        } else if (this.ruleForm.checkPass !== "") {
          this.$refs.ruleForm.validateField("checkPass");
        }
        callback();
      }
    };
    var validatePass2 = (rule, value, callback) => {
      if (value === "") {
        callback(new Error(this.$t("enter_new_pass_again")));
      } else if (value !== this.ruleForm.pass) {
        callback(new Error(this.$t("pass_mismatch")));
      } else {
        callback();
      }
    };
    return {
      ruleForm: {
        pass: "",
        checkPass: "",
        old_pass: ""
      },
      rules: {
        pass: [{ validator: validatePass, trigger: "blur" }],
        checkPass: [{ validator: validatePass2, trigger: "blur" }],
        old_pass: [{ validator: validateOldPass, trigger: "blur" }]
      },
      file_key: "",
      myEmail: "",
      myData: {
        image: "null",
        username: "null",
        desc: "null",
        email: "null"
      },
      userData: {
        desc: "null",
        email: "null",
        image: "null",
        pubkey: "null",
        username: "null"
      },
      url: "",
      myName: "",
      isNameEdit: false,
      ifupdate: false,
      mounseMark: false,
      loading: true
    };
  },
  created() {
    if (this.$route.params.id == "me") {
      this.getMyData();
    }
    if (this.$route.params.id != "me") {
      this.getUserData();
    }
  },
  mounted() {},
  methods: {
    getShopImages(url, status) {
      this.url = url;
      this.ifupdate = status;
    },
    faceMouseOver(b) {
      this.mounseMark = b;
    },
    // sub() {
    //   this.loading = true;
    //   var formObj = new FormData(document.getElementById("form1"));
    //   this.axios({
    //     method: "post",
    //     url: "be/helper/upload_image.do",
    //     data: formObj,
    //     processData: false,
    //     contentType: false
    //   })
    //     .then(res => {
    //       if (res.data.status == "SUCCEED") {
    //         this.file_key = res.data.data.file_key;
    //         this.axios({
    //           method: "post",
    //           url: "be/user/changephoto.do",
    //           data: { file_key: this.file_key }
    //         }).then(res => {
    //           this.getMyData();
    //           this.loading = false;
    //           this.$message({
    //             message: "上传成功！",
    //             type: "success"
    //           });
    //         });
    //       } else {
    //         this.$message.error("请选择要上传的头像!");
    //         this.loading = false;
    //       }
    //     })
    //     .catch(err => {
    //       this.$message.error("上传头像图片大小不能超过 2MB!");
    //       this.loading = false;
    //     });
    // },
    bindEmail() {
      this.axios({
        method: "post",
        url: "be/user/changeemail.do",
        data: { new_email: this.myEmail + "" }
      }).then(res => {
        if (res.data.status === "FAILED") {
          this.$message({
            message: this.$t("invalid_email"),
            type: "warning"
          });
        } else {
          this.$message({
            message: this.$t("bind_succeed"),
            type: "success"
          });
        }
      });
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          // this.$message({
          //   message: "提交成功！",
          //   type: "success"
          // });
          this.changePass();
        } else {
          this.$message({
            message: this.$t("check_form"),
            type: "warning"
          });
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    open1() {
      this.$message("这是一条消息提示");
    },
    open2() {
      this.$message({
        message: this.$t("update_succeed"),
        type: "success"
      });
    },

    open3() {
      this.$message({
        message: this.$t("check_form"),
        type: "warning"
      });
    },

    open4() {
      this.$message.error("错了哦，这是一条错误消息");
    },
    getMyData() {
      this.loading = true;
      // 现有的登录机制存在问题，
      // 可能本地判断已登录实际并没有登录而进入了 user 界面，这时没有数据渲染，需要让他跳回登录界面
      this.axios({
        method: "post",
        url: "/be/user/myprofile.do",
        data: {},
        withCredentials: true
      })
        .then(res => {
          if (res.data.status == "ERROR") {
            // 火狐浏览器有 BUG 暂时先这样跳，等 cookie 登陆做完后再在 user 页面判断。
            // this.$router.push("/login");
          }
          if (res.data.status == "SUCCEED") {
            this.myData = res.data.data.profile;
          }
          this.loading = false;
        })
        .catch(err => {});
    },
    getUserData() {
      this.axios({
        method: "post",
        url: "be/user/profile.do",
        data: { uid: this.$route.params.id }
      }).then(res => {
        this.userData = res.data.data.profile;
        this.loading = false;
      });
    },
    changeDesc() {
      if (this.myData.desc.length > 2000) {
        this.$message({
          message: this.$t("desc_long"),
          type: "warning"
        });
        return;
      }
      this.axios({
        method: "post",
        url: "be/user/changedesc.do",
        data: {
          desc: this.myData.desc
        }
      }).then(res => {
        this.open2();
      });
    },

    setUserName() {
      this.axios({
        method: "post",
        url: "be/user/changename.do",
        data: {
          name: this.myName
        }
      }).then(res => {
        if (res.data.status === "FAILED") {
          if (res.data.data.reason === "USER_ALREADY_EXIST") {
            this.$message({
              message: this.$t("user_exist"),
              type: "warning"
            });
          } else if (res.data.data.reason === "NAME_LENGTH") {
            this.$message({
              message: this.$t("user_length"),
              type: "warning"
            });
          } else {
            this.$message.error(this.$t("update_failed"));
          }
        }
        if (res.data.status === "SUCCEED") {
          this.open2();
          this.getMyData();

          this.$store.commit("getUserName", this.myName);
        }
      });
    },

    islSetUserName(b) {
      this.isNameEdit = b;
      this.$forceUpdate();
      // this.isNameEdit === b;
    },
    changePass() {
      this.axios({
        method: "post",
        url: "be/user/changepass.do",
        data: {
          old_pass: this.ruleForm.old_pass + "",
          new_pass: this.ruleForm.pass + ""
        }
      }).then(res => {
        if (res.data.status == "FAILED") {
          this.$message.error(this.$t("wrong_pass"));
        } else {
          this.open2();
        }
      });
    }
  },
  watch: {
    ifupdate(n) {
      if (n === true) {
        this.getMyData();
      }
    },
    $route(n) {
      if (n.fullPath === "/users/me") {
        this.getMyData();
      }
    }
  },
  components: { AppCropper }
};
</script>

<style scoped lang="less">
.el-form-item {
  margin-bottom: 20px !important;
}
.wave {
  position: absolute;
  transform: translate(-18%, -18%);
  width: 400px;
  height: 400px;
  text-align: center;
  line-height: 100px;
  font-size: 28px;
}
.wave .circle {
  position: absolute;
  border-radius: 50%;
  opacity: 0;
}

/* 波纹效果 */
.wave.ripple .circle {
  width: calc(100% - 6px); /* 减去边框的大小 */
  height: calc(100% - 6px); /* 减去边框的大小 */
  border: 3px solid #fff;
}

.animeActive1 {
  animation: circle-opacity 2s infinite;
}
.animeActive2 {
  animation: circle-opacity 2s infinite;
  animation-delay: 0.3s;
}
.animeActive3 {
  animation: circle-opacity 2s infinite;
  animation-delay: 0.6s;
}

.wave.ripple.danger {
  color: red;
}

.wave.ripple.danger .circle {
  border-color: red;
}

.wave.ripple.warning {
  color: orange;
}

.wave.ripple.warning .circle {
  border-color: orange;
}

/* 波动效果 */
.wave.solid .circle {
  width: 100%;
  height: 100%;
  background: #fff;
}

.wave.solid .circle:first-child {
  /* animation: circle-opacity 2s; */
}

.wave.solid.danger {
  color: red;
}

.wave.solid.danger .circle {
  background: red;
}

.wave.solid.warning {
  color: orange;
}

.wave.solid.warning .circle {
  background: orange;
}

@keyframes circle-opacity {
  from {
    opacity: 1;
    transform: scale(0);
  }
  to {
    opacity: 0;
    transform: scale(1);
  }
}
.el-icon-edit {
  font-size: 19px;
  cursor: pointer;
}
.el-icon-edit:hover {
  transition: all 0.3s ease;
  color: #1b9af7;
}
.face2 {
  position: relative;
  left: 30%;
  width: 256px;
  height: 256px;
  border-radius: 50%;
  overflow: hidden;
  margin-bottom: 20px;
  transition: all 0.3s ease;
  box-shadow: 0px 0px 1px 1px #fbfcfd;
  &:hover {
    transform: scale(1.1);
  }
  img {
    width: 256px;
    height: 256px;
    transform: scale(1.1);
    /* position: absolute; */
    /* top: 50%; */
    /* left: 50%; */
    transition: all 0.3s ease;
    &:hover {
      transform: scale(1);
    }
  }
}
.bg {
  /* background: url("../static/img/imoto_left.jpg") no-repeat; */
  background-size: 100% 100%;
}

.email-info {
  margin-bottom: 20px;
}
.email > .el-input {
  margin-bottom: 20px;
}

.left-content {
  top: 18%;
  left: 37%;
  width: 73%;
  position: relative;

  p {
    width: 255px;
    height: 30px;
    margin-top: 30px;
    text-align: center;
  }
  .face {
    width: 256px;
    height: 256px;
    border-radius: 50%;
    overflow: hidden;

    transition: all 0.3s ease;
    box-shadow: 0px 0px 1px 1px #fbfcfd;

    /* transform: translate(-50%,-50%); */
    &:hover {
      transform: scale(1.1);
    }

    img {
      width: 256px;
      height: 256px;
      transform: scale(1.1);
      /* position: absolute; */
      /* top: 50%; */
      /* left: 50%; */
      transition: all 0.3s ease;
      &:hover {
        transform: scale(1);
      }
    }
  }
}

.post :nth-child(2) {
  position: absolute;
  right: 0px;
}

.el-form {
  width: 450px;
}

.bigbox {
  height: 1000px;
  display: flex;
  background-color: white;
  opacity: 0.9;
}

.bigbox_left {
  position: absolute;

  width: 50%;
  max-width: 800px;
  height: 900px;
  // background: url("../static/img/imoto_left.jpg") no-repeat;
  // background-size: 100% 100%;
  // background-color: #d5d5d5;
}

.bigbox_right {
  position: absolute;
  right: 0px;
  padding: 50px;
  padding-top: 150px;
  text-align: left;
  margin: auto;
  width: 40%;
  height: 900px;
}

.desc_name {
  margin-bottom: 20px;
}

.bigbox_right .desc {
  width: 100%;
  height: 250px;
  font-size: 18px;
}

.desc_name {
  color: #333;
  font-family: Arial, Helvetica, sans-serif;
}

.bigbox button {
  width: 100px;
  height: 40px;
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  background: #fff;
  border: 1px solid #c4c4c4;
  color: #1f2d3d;
  margin: 0;
  padding: 10px 15px;
  border-radius: 4px;
}

.text-form {
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 100px;
  padding: 10px;
  font-size: 16px;
  color: #4a4a4a;
  border: 1px solid #d1d1d1;
  border-radius: 2px;
}

.psd-form input {
  width: 300px;
  height: 30px;
  margin-bottom: 20px;
  padding-left: 10px;
  display: block;
  font-size: 16px;
  color: #4a4a4a;
  border: 1px solid #d1d1d1;
  border-radius: 2px;
}

#imoto2 {
  position: absolute;
  display: none;
  width: 50%;
  height: 800px;
  background: url("../static/img/imoto_left2.jpg") no-repeat;
  background-size: 100% 100%;
}
</style>