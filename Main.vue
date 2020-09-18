<template>
  <el-container>
    <el-header>
      <!-- header -->
      <div class="head_box" id="step1">
        <div class="width100">
          <div class="logo_box">
            <img src="@/assets/images/title.png" alt />
          </div>
          <!-- 滚动公告 -->
          <div class="shell" id="step2">
            <div class="textBox">
              <i class="iconfont icongonggao"></i>
              <vue-seamless-scroll
                :data="listData"
                :class-option="optionHover"
                class="seamless-warp"
              >
                <ul class="item">
                  <li v-for="(item,index) in listData" :key="item">
                    <span class="date" v-text="item.date"></span>
                    <span class="title" v-text="item.title" :title="item.title"></span>
                    <img v-if="index===0" src="@/assets/images/new.png" class="new" />
                  </li>
                </ul>
              </vue-seamless-scroll>
            </div>
            <div class="view" @click="showNoticeDialog = true">
              <i class="iconfont icongengduo"></i>
            </div>
          </div>
          <div class="login">
            <ul>
              <li @click="open" id="step9">
                <i class="iconfont iconzhuxiao"></i>
              </li>
              <li class="role_box" id="step8">
                <img src="@/assets/images/user.png" alt class="role" />
                <span :title="currentUserName">{{currentUserName}}</span>
              </li>
              <li class="time_box" id="step7">
                <i class="iconfont iconshijian"></i>
                <span>{{gettime}}</span>
              </li>
              <li @click="toggleFullScreen" id="step6">
                <!-- 非全屏 -->
                <i v-if="!fullscreen" class="iconfont iconquanpingxianshi"></i>
                <!-- 全屏 -->
                <i v-else class="iconfont iconshousuo"></i>
              </li>
              <li @click="openChatDialog" id="step5">
                <i class="iconfont iconxiaoxi-copy"></i>
                <el-badge is-dot class="badge-item" v-if="showBadge"></el-badge>
              </li>
              <li class="zb" @click="showVideoDialog=true" id="step4">
                <i class="iconfont iconshipin"></i>
                <i class="iconfont iconLIVE"></i>
              </li>
              <li @click="guide" id="step3">
                <i class="iconfont iconxinshouyindao"></i>
                <span>帮助</span>
              </li>
              <div class="clear"></div>
            </ul>
          </div>
          <div class="clear"></div>
        </div>
      </div>
      <!-- header end -->
      <!-- 公告弹窗 -->
      <el-dialog title="系统公告" :visible.sync="showNoticeDialog" class="notice-dialog">
        <div class="txt">
          <ul>
            <li v-for="item in listData" :key="item">
              <a href="#">
                <div class="title" :title="item.title">{{item.date}} {{item.title}}</div>
              </a>
            </li>
          </ul>
        </div>
      </el-dialog>
      <!-- 公告弹窗 end -->
      <!-- 开标视频弹窗 -->
      <el-dialog title="开标视频直播" :modal="false" :visible.sync="showVideoDialog">
        <p>暂无视频</p>
      </el-dialog>
      <!-- 开标视频弹窗 end-->
      <!--聊天窗口-->
      <chat ref="chat" :visible="showChatDialog" id="chat-window-id" title="开标会议室即时通讯窗口"></chat>
      <!--聊天窗口 end-->
    </el-header>
    <el-container :style="defaultHeight">
      <el-aside>
        <!-- 左侧导航展开状态 -->
        <div class="side_box" v-if="openLeftNavigation" id="step11">
          <div class="nav_item" @click="openLeftNavigation=false" id="step12">
            <img src="@/assets/images/side0.png" alt />
          </div>
          <ul class="nav_one">
            <li
              v-for="(nav,index) in navigations"
              :key="nav.id"
              :class="{'active':nav.active}"
              @click="onClickNavigation(nav.id)"
              :id="forId(index)"
            >
              <i :class="nav.img"></i>
              <p>{{nav.title}}</p>
            </li>
          </ul>
        </div>
        <!-- 左侧导航展开状态 end -->
        <!-- 左侧导航收缩状态 -->
        <div class="ss_side_box" v-if="!openLeftNavigation" id="step11">
          <div class="nav_item" @click="openLeftNavigation=true" id="step12">
            <img src="@/assets/images/side1.png" alt />
          </div>
          <ul class="nav_one">
            <li
              v-for="(nav,index) in navigations"
              :key="nav.id"
              :class="{'active':nav.active}"
              @click="onClickNavigation(nav.id)"
              :id="forId(index)"
            >
              <i :class="nav.img"></i>
            </li>
          </ul>
        </div>
        <!-- 左侧导航收缩状态 end -->
      </el-aside>
      <el-main>
        <!-- 标题框 -->
        <div class="nav_box" id="step16">
          <ul>
            <!-- 左侧导航名称显示 -->
            <li @click="onClickNavigation(selectedNavigation.id)">
              <i :class="selectedNavigation.img"></i>
              <span class="cur">{{selectedNavigation.title}}</span>
            </li>
            <!-- 流程节点名称显示 -->
            <li v-if="selectedFlowNode">
              <span>/ &nbsp; {{selectedFlowNode.itemName}}</span>
            </li>
          </ul>
        </div>
        <!-- 标题框 end-->
        <div class="content_bg">
          <!-- 提示信息 -->
          <el-alert
            title="注意：系统平台如果文字或者提示语若有偏差，请以招标文件、答疑文件等相关文件为准。"
            type="warning"
            show-icon
            center
          ></el-alert>
          <!-- 提示信息 end -->
          <router-view :key="refreshKey"></router-view>
        </div>
      </el-main>
    </el-container>
    <el-footer>
      <footer>
        <div class="footer_box">
          <p>
            ©福建首众信息科技有限公司-技术支持
            &nbsp;&nbsp;&nbsp;&nbsp; 客服热线：400-999-6901
            &nbsp;&nbsp;&nbsp;&nbsp; QQ在线咨询：
            <span>点击咨询</span>
          </p>
        </div>
      </footer>
    </el-footer>
  </el-container>
</template>

<script>
import screenfull from "screenfull";
import vueSeamlessScroll from "vue-seamless-scroll";
import chat from "@/components/ChatWindow";
import Driver from "driver.js"; // import driver.js
import "driver.js/dist/driver.min.css"; // import driver.js css
import steps from "@/js/guide";
export default {
  name: "Main",
  components: {
    // 引入无缝滚动插件
    vueSeamlessScroll,
    // 引入聊天窗口
    chat,
  },
  data() {
    return {
      //引导页默认值
      driver: null,
      // 滚动公告内容
      listData: [
        {
          date: "2019-09-10 17:00:00",
          title: "[第一数字信封评审结果]环节开始啦！",
        },
        {
          date: "2019-09-09 17:00:00",
          title:
            "[第一数字信封评审结果]环节开始啦开始啦开始啦开始啦开始啦开始啦!",
        },
        {
          date: "2019-09-08 17:00:00",
          title:
            "[第一数字信封评审结果]环节开始啦开始啦开始啦开始啦开始啦开始啦!",
        },
      ],
      // 左侧导航列表
      navigations: [
        {
          // id
          id: 1,
          // 标题
          title: "开标流程",
          // 图标
          img: "iconfont iconliucheng",
          // 是否选中状态
          active: true,
          // 路由地址
          url: "/process",
          visible: true,
        },
        {
          id: 2,
          title: "项目信息",
          img: "iconfont iconxiangmuxinxi",
          url: "/tpinfo",
          active: false,
          visible: true,
        },
        {
          id: 3,
          title: "封标",
          img: "iconfont iconfengjin",
          active: false,
          url: "/sealed",
          visible: true,
        },
      ],
      // 显示公告窗口
      showNoticeDialog: false,
      // 当前时间
      gettime: "",
      // 当前用户的名称
      currentUserName: "投标测试企业九投标测试企业九投标测试企业九",
      // 开标视频
      showVideoDialog: false,
      // 左侧导航是否张开
      openLeftNavigation: true,
      // 默认高度
      defaultHeight: {
        height: "",
      },
      // 显示聊天窗口
      showChatDialog: false,
      // 显示新消息提示
      showBadge: true,
      // 是否全屏显示
      fullscreen: false,
      // 选中的导航
      selectedNavigation: null,
      // 选中的流程节点
      selectedFlowNode: null,
    };
  },
  created() {
    //页面创建时执行一次getHeight进行赋值，顺道绑定resize事件
    window.addEventListener("resize", this.getHeight);
    this.getHeight();
  },
  beforeMount() {
    // 初始化选默认选中的节点,这个必须在mount之前做,否者页面会报错
    this.selectedNavigation = this.navigations[0];
    this.$router.push(this.selectedNavigation.url);
  },
  mounted() {
    // 引导页初始化
    this.driver = new Driver({
      className: "scoped-class", // className to wrap driver.js popover
      animate: true, // Animate while changing highlighted element
      opacity: 0.75, // Background opacity (0 means only popovers and without overlay)
      padding: 10, // Distance of element from around the edges
      allowClose: true, // Whether clicking on overlay should close or not
      overlayClickNext: false, // Should it move to next step on overlay click
      doneBtnText: "完成", // Text on the final button
      closeBtnText: "关闭", // Text on the close button for this step
      nextBtnText: "下一步", // Next button text for this step
      prevBtnText: "上一步", // Previous button text for this step
      // Called when moving to next step on any step
    });
    // 调用当前时间
    this.currentTime();
    // 监听全屏窗口大小改变，screenfull.isFullscreen的值为是否全屏，若是则true，反之false
    if (screenfull.isEnabled) {
      screenfull.on("change", () => {
        this.fullscreen = screenfull.isFullscreen;
        // console.log("Am I fullscreen?", screenfull.isFullscreen ? "Yes" : "No");
      });
    }
  },
  computed: {
    // 滚动公告计算
    optionHover() {
      return {
        hoverStop: true, // 鼠标悬停停止滚动
        direction: 0, // 0向下 1向上 2向左 3向右
        step: 0.4, // 滚动速度
        singleHeight: 20, // 滚动单行
        waitTime: 2500, // 单行停顿时间
        limitMoveNum: 1, // 开始无缝滚动的数据量 this.dataList.length
      };
    },
  },
  methods: {
    // 新手引导
    guide() {
      this.driver.defineSteps(steps);
      this.driver.start();
    },
    // 引导侧边栏索引
    forId: function (index) {
      return "step" + parseInt(index +13);
    },
    // 获取当前时间
    currentTime() {
      setInterval(this.getTime, 500);
    },
    getTime: function () {
      var _this = this;
      let yy = new Date().getFullYear();
      let mm = new Date().getMonth() + 1;
      let dd = new Date().getDate();
      let hh = new Date().getHours();
      let mf =
        new Date().getMinutes() < 10
          ? "0" + new Date().getMinutes()
          : new Date().getMinutes();
      let ss =
        new Date().getSeconds() < 10
          ? "0" + new Date().getSeconds()
          : new Date().getSeconds();
      _this.gettime =
        yy +
        "年" +
        mm +
        "月" +
        dd +
        "日" +
        " " +
        hh +
        "时" +
        mf +
        "分" +
        ss +
        "秒";
    },
    // 获取高度减去头尾
    getHeight() {
      this.defaultHeight.height = window.innerHeight - 94 + "px";
    },
    // 打开聊天窗口
    openChatDialog() {
      this.showChatDialog = true;
      this.showBadge = false;
    },
    // 切换全屏
    toggleFullScreen() {
      if (!screenfull.isEnabled) {
        this.$message({
          message: "您的浏览器无法进入全屏模式",
          type: "warning",
        });
        return false;
      }
      screenfull.toggle();
    },
    // 点击导航
    onClickNavigation(id) {
      // 设置没有选中任何流程节点
      this.selectedFlowNode = null;
      let nav;
      for (let i = 0; i < this.navigations.length; i++) {
        nav = this.navigations[i];
        nav.active = false;
        if (nav.id === id) {
          nav.active = true;
          this.selectedNavigation = nav;
          this.$router.push(nav.url);
        }
        this.$set(this.navigations, i, nav);
      }
    },
    // 跳转到流程节点页
    goProcessPage(url) {
      this.$router.push(url);
    },
    // 进入流程节点
    goFlowNode(node) {
      let status = node.status;
      // 尚未开始
      if (status < 2) {
        return;
      }
      // 修改主页的选中节点
      this.selectedFlowNode = node;
      // 把左侧导航的选中状态去掉
      this.selectedNavigation.active = false;

      // 投标人路径
      let url = node.bidder_page_URL;
      // 如果是招标人或者代理
      if (node.isTenderer) {
        url = node.tender_page_url;
      }
      this.$router.push({
        // 这里一定要用name,否者vue无法传递参数
        name: url,
        // 节点信息作为参数
        params: { fnode: node },
      });
      console.log(this);
    },
    // 退出
    open() {
      this.$confirm("确定要退出开标辅助系统吗??", "操作提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {})
        .catch(() => {});
    },
  },
};
</script>


<style>
@import "../assets/css/Reset.css";
@import "../assets/css/common.css";
/* 重置 */
.el-header,
.el-footer,
.el-main {
  padding: 0 !important;
  height: auto !important;
}
.el-aside {
  width: auto !important;
}
.el-alert__title,
.el-alert__icon {
  color: #e6a23c;
}
.el-message-box__btns button:nth-child(2) span {
  color: white;
}
.badge-item {
  float: right;
}
.el-badge__content {
  border: none;
  background-color: red;
}
div#driver-highlighted-element-stage {
  background-color: rgba(255, 255, 255, 0.9) !important;
}

/* 重置end */
/* 公告弹窗 */
.notice-dialog .txt ul {
  margin: 20px;
}
.notice-dialog .txt li {
  list-style-type: square;
  margin-bottom: 16px;
}
.notice-dialog .txt .title {
  float: left;
  font-size: 16px;
  color: #646465;
  max-width: 600px;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
.notice-dialog .txt .title:hover {
  color: #007aff;
}
.notice-dialog .txt .time {
  float: right;
  font-size: 14px;
  color: #b4b6b8;
}
.el-dialog__header {
  text-align: left;
  border-bottom: 1px solid #c8c7c7;
  margin: 0 20px;
  padding-left: 0px;
  padding-right: 0px;
}
.el-dialog__title {
  color: #007aff;
  border-left: 4px solid #007aff;
  padding-left: 10px;
}
/* 公告弹窗 end */
</style>
