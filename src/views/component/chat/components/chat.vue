<template>
  <div id="app">
    <div class="sidebar">
      <div class="card">
        <header>
          <img class="avatar" width="40" height="40" :alt="name" :src="avatar">
          <p class="name">{{name}}</p>
        </header>
        <footer>
          <input class="search" type="text" placeholder="search user...">
        </footer>
      </div>
      <div class="list">
        <ul>
          <li :class="{ active: current_window_id === 0 }" @click="selectWindow(0)">
            <img class="avatar" width="30" height="30" src="/avatar/group.png">
            <p class="name">官方群组</p>
          </li>
          <li v-for="item in userList" :class="{ active: current_window_id === item.id }" @click="selectWindow(item.id)">
            <img class="avatar" width="30" height="30" :alt="item.name" :src="item.avatar">
            <p class="name">{{item.name}}</p>
          </li>
        </ul>
      </div>
    </div>
    <div class="main">
      <div class="message">
        <ul>
          <li>
            <p class="time">
              <span>2018-01-01</span>
            </p>
            <div class="main">
              <img class="avatar" width="30" height="30" src="http://cdn.tycoding.cn/author.png" />
              <span class="main-name">Tumo</span>
              <div class="text">我是Tumo</div>
            </div>
          </li>
          <li>
            <p class="time">
              <span>2018-01-01</span>
            </p>
            <div class="main self">
              <img class="avatar" width="30" height="30" src="http://cdn.tycoding.cn/author.png" />
              <span class="main-name">TyCoding</span>
              <div class="text">我是TyCoding</div>
            </div>
          </li>
        </ul>
      </div>
      <div class="text">
        <textarea placeholder="按 Enter 键发送"></textarea>
        <div class="btn">
          <el-button size="mini" type="success">清空</el-button>
          <el-button size="mini" type="success">发送</el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  export default {
    name: "chat",
    computed: {
      ...mapGetters([
        'id',
        'name',
        'avatar'
      ])
    },
    data() {
      return {
        //当前激活窗口ID
        current_window_id: 0,
        userList: []
      }
    },
    created() {
      this.init();
    },
    methods: {
      _notify(message, type) {
        this.$message({
          message: message,
          type: type
        })
      },

      init() {
        //加载官方群组窗口
        var websocket = new WebSocket('ws://localhost:5002/chat/' + this.id)
        websocket.onerror = function() {
          console.log('链接错误')
        }
        websocket.onopen = function() {
          console.log('链接成功')
          websocket.send('Hi my name is tycoding')
        }

        //加载在线用户列表
        let list = [
          {id: 1, name: 'tycoding', avatar: '/avatar/20180414165754.jpg'},
          {id: 2, name: 'tumo', avatar: '/avatar/20180414165815.jpg'},
          {id: 3, name: 'user', avatar: '/avatar/20180414165821.jpg'},
        ]
        this.userList = list
      },

      //切换选择窗口
      selectWindow(id) {
        this.current_window_id = id;
      },

      send(form) {
        this.$refs[form].validate((valid) => {
          if (valid) {

          } else {
            console.log('error submit!!');
            return false;
          }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  #app {
    margin: 20px auto;
    width: 800px;
    height: 600px;
    overflow: hidden;
    border-radius: 3px;
    .sidebar, .main {
      height: 100%;
    }
    .sidebar {
      float: left;
      width: 200px;
      color: #f4f4f4;
      background-color: #2e3238;
    }
    .main {
      position: relative;
      overflow: hidden;
      background-color: #eee;
    }
    .text {
      /*position: absolute;*/
      /*width: 100%;*/
      bottom: 0;
      left: 0;
    }
    .message {
      height: calc(100% - 160px);
    }
  }
  .list {
    ul{
      margin: 0;
      padding: 0;
    }
    li {
      list-style:none;
      padding: 12px 15px;
      border-bottom: 1px solid #292C33;
      cursor: pointer;
      transition: background-color .1s;
      &:hover {
        background-color: rgba(255, 255, 255, 0.03);
      }
      &.active {
        background-color: rgba(255, 255, 255, 0.1);
      }
    }
    .avatar, .name {
      vertical-align: middle;
    }
    .avatar {
      border-radius: 2px;
    }
    .name {
      display: inline-block;
      margin: 0 0 0 15px;
    }
  }
  .card {
    padding: 12px;
    border-bottom: solid 1px #24272C;
    footer {
      margin-top: 10px;
    }
    .avatar, .name {
      vertical-align: middle;
    }
    .avatar {
      border-radius: 2px;
    }
    .name {
      display: inline-block;
      margin: 0 0 0 15px;
      font-size: 16px;
    }
    .search {
      padding: 0 10px;
      width: 100%;
      font-size: 12px;
      color: #fff;
      height: 30px;
      line-height: 30px;
      border: solid 1px #3a3a3a;
      border-radius: 4px;
      outline: none;
      background-color: #26292E;
    }
  }
  .message {
    padding: 10px 15px;
    overflow-y: scroll;
    ul{
      margin-left: -40px;
    }
    li {
      list-style:none;
      margin-bottom: 15px;
    }
    .time {
      margin: 7px 0;
      text-align: center;
      > span {
        display: inline-block;
        padding: 0 18px;
        font-size: 12px;
        color: #fff;
        border-radius: 2px;
        background-color: #dcdcdc;
      }
    }
    .avatar {
      float: left;
      margin: 0 10px 0 0;
      border-radius: 3px;
    }
    .main-name{
      font-size: 11px;
      color: gray;
      display: inherit;
      font-weight: 500;
      margin-bottom: 5px;
    }
    .text {
      height: auto !important;
      display: inline-block;
      position: relative;
      padding: 0 10px;
      max-width: calc(100% - 40px);
      min-height: 30px;
      line-height: 2.5;
      font-size: 12px;
      text-align: left;
      word-break: break-all;
      background-color: #fafafa;
      border-radius: 4px;
      &:before {
        content: " ";
        position: absolute;
        top: 9px;
        right: 100%;
        border: 6px solid transparent;
        border-right-color: #fafafa;
      }
    }
    .self {
      text-align: right;
      .avatar {
        float: right;
        margin: 0 0 0 10px;
      }
      .text {
        background-color: #b2e281;
        &:before {
          right: inherit;
          left: 100%;
          border-right-color: transparent;
          border-left-color: #b2e281;
        }
      }
    }
  }
  .text {
    border: 1px solid #e6ebf5;
    height: 160px;
    background: white;
    border-top: solid 1px #ddd;
    textarea {
      padding: 10px;
      height: 80%;
      width: 100%;
      border: none;
      outline: none;
      font-family: "Micrsofot Yahei";
      resize: none;
    }
    .btn{
      float: right;
      margin: -2px 9px;
    }
  }
</style>
