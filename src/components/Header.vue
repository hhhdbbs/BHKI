<template>
  <!-- 公共头部组件 -->
  <div class="header">

    <!-- 系统logo -->
    <div class="logo" >
      <img src="https://fileserver.aminer.cn/sys/aminer/product/AMinerIcon_Conf.png" height="70" width="70">
    </div>
    <div class="title" style="font-weight: bold"><p>BHKI</p></div>
    <div class="daohang" data-click="{'act_block':'newlinks','fm':'beha'}" style="display: block;"> 
      <label class="btn btn-info active" style="margin-right:60px;">    
        <a href="./" target="_blank" class="menuitem" >
          <el-button type="primary" icon="el-icon-s-home">主页</el-button>
        </a>
      </label>

      <label class="btn btn-info active" style="margin-right:60px;">    
        <a href=" " target="_blank" class="menuitem" >
          <el-button type="primary" icon="el-icon-document-copy">帖子广场</el-button>
        </a>
      </label>

      <label class="btn btn-info active" style="margin-right:60px;">    
        <a href=" " target="_blank" class="menuitem" >
          <el-button type="primary" icon="el-icon-search">检索</el-button>
        </a>
      </label>

      <label class="btn btn-info active" style="margin-right:60px;">    
        <a href="./welcome" target="_blank" class="menuitem" >
          <el-button type="primary" icon="el-icon-magic-stick">欢迎</el-button>
        </a>
      </label>

      <label v-if="isLogin" class="btn btn-info active" style="margin-right:60px;">    
        <a href="./welcome" target="_blank" class="menuitem" >
          <el-button type="primary" icon="el-icon-place">查看门户</el-button>
        </a>
      </label>
    </div>

    <!-- 头部组件右边功能区 -->
    <div class="header-right">
      <!--从这里开始写右边功能区-->

      <!--tooltip提供了两个主题：dark和light，通过 effect 设置主题 -->
      <!-- 通过三元表达式来设置不同的文字提示，placement属性控制文字提示出现的位置 -->
      <div class="btn-fullscreen" @click="handleFullScreen">
        <el-tooltip effect="light" :content="fullscreen?`取消全屏`:`全屏`" placement="bottom">
          <i class="el-icon-sort"></i>
        </el-tooltip>
      </div>

      <!--这里是未登录看到的登录/注册链接-->
      <div v-if="!isLogin">
        <el-link :underline="false" href="./Home">登录</el-link>
        /
        <el-link :underline="false" href="./register">注册</el-link>
      </div>
      <!--登录注册链接到此结束-->

      <!--从这里开始是登录后才能看到的消息提示和用户头像-->
      <div v-if="isLogin">
        <!--这里是消息提示-->

        <el-tooltip
                effect="light"
                :content="notread?`有${notread}条未读消息`:`消息中心`"
                placement="bottom"
        >
          <div class="btn-bell">
            <span class="btn-bell-badge" v-if="notread"></span>
            <el-popover style="outline:0!important" class="bell" placement="bottom" width="400" trigger="click">
              <el-tabs type="border-card">
                <el-tab-pane label="未读">
                  <el-table :data="de_tableData" height="500" stripe>
                    <el-table-column prop="time" label="时间" width="70"></el-table-column>
                    <el-table-column prop="message_a" label="信息" width="180"></el-table-column>
                    <el-table-column prop="operate" label="操作" width=auto>
                      <template slot-scope="scope">
                        <p v-if="scope.row.operate=='1'">
                          <el-button @click="agree(scope.row)" type="text" size="small">同意</el-button>
                          <el-button @click="disagree(scope.row)" type="text" size="small">拒绝</el-button>
                        </p>
                        <p v-if="scope.row.operate=='0'">
                          <el-button @click="read(scope.row)" type="text" size="small">标记已读</el-button>
                        </p>
                      </template>
                    </el-table-column>
                  </el-table>
                </el-tab-pane>
                <el-tab-pane label="已读">
                  <el-table :data="tableData" height="500" stripe>
                    <el-table-column prop="time" label="时间" width="70"></el-table-column>
                    <el-table-column prop="message_a" label="信息" width="180"></el-table-column>
                    <el-table-column prop="operate" label="操作" width=auto>
                      <template slot-scope="scope">
                        <p v-if="scope.row.operate=='2'">已同意</p>
                        <p v-if="scope.row.operate=='3'">已拒绝</p>
                      </template>
                    </el-table-column>
                  </el-table>
                </el-tab-pane>
              </el-tabs>
              <el-button slot="reference" class="el-icon-message-solid"></el-button>
            </el-popover>
          </div>
        </el-tooltip>
        <!-- 通过对message的判定，来决定是否显示小红点 -->
        <!-- <span class="btn-bell-badge" v-if="message"></span> -->

        <!--消息提示到此结束-->
        <!-- </el-col>
        <el-col :span="12" offset="10"> -->
        <!--这里是用户头像-->
        <div class="avator">
          <el-dropdown class="user-name" trigger="hover" @command="handleCommand">
            <div class="block">
              <el-avatar :size="40" :src="userAvator"></el-avatar>
            </div>
            <!-- slot设置下拉列表 -->
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item command="myinfo">你好,{{username}}</el-dropdown-item>
              <el-dropdown-item command="editinfo">修改信息</el-dropdown-item>
              <el-dropdown-item command="loginout">退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </div>
        <!--用户头像到此结束-->
        <!-- </el-col>
      </el-row> -->
      </div>
      <!--右边功能区到此结束-->
    </div>
  </div>
</template>

<script>
  // 加载bus实现组件通信
  //import bus from "../../utils/bus";
  // import axios from ""

  export default {
    name: 'Header',
    data() {
      return {
        collapse: false,
        fullscreen: false,
        userid: 0,
        username: "还没有用户登录",
        userAvator:
            this.global.baseUrl +
            "/image/avatar/show?user_id=" +
            this.$store.state.user.username.id,
        isLogin : false,

        tableData: null,
        de_tableData: null,
        notread: 0,
        now: new Date(),
      };
    },
  

    methods: {
      // 用户名下拉菜单选择事件
      handleCommand(command) {
        //退出登录事件
        if (command == "loginout") {
          // 从本地存储中删除用户名
          // localStorage.removeItem("username");
          this.$store.commit("logout");
          this.isLogin = false;
          this.userid = 0;
          this.username = "还没有用户登录";
          this.userAvator =
              "https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png";
          // 跳转到登录页面
          this.$router.push("/login");
        } else if (command == "myinfo") {
          this.$router.push({name:'DashBoard',params:{user_id:this.$store.state.user.username.id}});
        } else if(command === 'editinfo'){
          this.$router.push('/personalinformation');
        }
      },

      // 侧边栏折叠
      collapseChage() {
        this.collapse = !this.collapse;
        // 通过bus发送信息
        bus.$emit("collapse", this.collapse);
      },

      // 全屏事件
      handleFullScreen() {
        // 获取文档对象的根元素
        let element = document.documentElement;
        if (this.fullscreen) {
          // 设置不同浏览器下的退出全屏模式
          // -webkit- 兼容chrome 和 safari
          // -moz- 兼容firefox
          // -ms- 兼容IE浏览器
          // -o- 兼容opera
          if (document.exitFullscreen) {
            document.exitFullscreen();
          } else if (document.webkitCancelFullScreen) {
            document.webkitCancelFullScreen();
          } else if (document.mozCancelFullScreen) {
            document.mozCancelFullScreen();
          } else if (document.msExitFullscreen) {
            document.msExitFullscreen();
          }
        } else {
          // 设置不同浏览器下的进入全屏模式
          if (element.requestFullscreen) {
            element.requestFullscreen();
          } else if (element.webkitRequestFullScreen) {
            element.webkitRequestFullScreen();
          } else if (element.mozRequestFullScreen) {
            element.mozRequestFullScreen();
          } else if (element.msRequestFullscreen) {
            // IE11
            element.msRequestFullscreen();
          }
        }
        this.fullscreen = !this.fullscreen;
      },

      checkLogin: function () {
        return localStorage.getItem("username") !== null;
      }

    },

    // 初始化页面完成后，对页面可见区域宽度进行判定，如果页面宽度小于1500，则触发 collapseChage 方法。
    mounted: function () {
      if (document.body.clientWidth < 1500) {
        this.collapseChage();
      }
      //通过 bus 通信获取 message 的值。
      bus.$on("msg", (e) => {
        this.message = e;
      });
    },
    created: function () {
      this.now = new Date()
      if (this.checkLogin()) {
        console.log("在构造函数中检测到用户已经登录");
        this.isLogin = true;
        this.username = this.$store.state.user.username.name;
        this.userid = this.$store.state.user.username.id;
        this.userAvator =
            this.global.baseUrl +
            "/image/avatar/show?user_id=" +
            this.$store.state.user.username.id;
      }

      this.f5();

    },
  };
</script>

<!-- CSS样式 -->
<style scoped>
  .header {
    position: relative;
    box-sizing: border-box;
    width: 100%;
    height: 70px;
    font-size: 22px;
    color: #000000;
    background-color: #ffffff;
  }

  .collapse-btn {
    float: left;
    padding: 0 21px;
    cursor: pointer;
    line-height: 70px;
  }

  .header .logo {
    float: left;
    width: 100px;
    line-height: 70px; 
  }

  .header .title {
    float: left;
    width: 80px;
    line-height: 40px;
  }

  .header .daohang {
    float: left;
    width: 1000px;
    line-height: 70px;
  }


  .header-right {
  
    line-height: 50px;
    float: right;
    padding-right: 50px;
    align-items: center;
    display: flex;
    transform: translate(0, 15%);
  }

  .header-user-con {
    display: flex;
    height: 70px;
    align-items: center;
  }

  .btn-fullscreen {
    height: 30px;
    width: 30px;
    border-radius: 15px;
    cursor: pointer;
    position: absolute;
    transform: rotate(45deg);
    left: -50%;
    margin-right: 5px;
    font-size: 24px;
  }

  .btn-bell {
    height: 30px;
    width: 20px;
    position: absolute;
    /* top: 50%;
    left: 90%;*/
    transform: translate(-150%, 30%);
  }

  /* .btn-fullscreen {
    text-align: center;
  } */
  .btn-bell-badge {
    position: absolute;
    right: 0;
    top: -2px;
    width: 8px;
    height: 8px;
    border-radius: 4px;
    background: #f56c6c;
    color: #fff;
  }

  .btn-bell .el-icon-bell {
    color: #fff;
  }

  .user-name {
    margin-top: 10px;
    margin-left: 10px;
  }

  .avator {
    margin-left: 20px;
  }

  .user-avator img {
    display: block;
    width: 40px;
    height: 40px;
    border-radius: 50%;
  }

  .el-dropdown-link {
    color: #000000;
    cursor: pointer;
  }

  .el-dropdown-menu__item {
    text-align: center;
  }

  .bell {
    background-color: transparent;
    border-color: transparent;
  }
</style>
