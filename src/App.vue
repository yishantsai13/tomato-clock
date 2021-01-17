<template>
  <div class="main">
    <div class="left">
      <div class="chat">
        <div class="chat-bubble">
          <Task
            v-if="hasPlayedTask"
            :content="playedTask.content"
            :isShowPlayIcon="false"
            :id="playedTask.id"
            :size="'big'"
            @taskDone="doneTask"
          ></Task>
          <div v-else>Wonderful! There's noting to do!</div>
          <div class="chat-bubble-tail"></div>
        </div>
      </div>
      <div class="task-list">
        <Task
          v-for="task in unDoTaskListUnplayed"
          :content="task.content"
          :key="task.id"
          :id="task.id"
          @taskDone="doneTask"
          @playTask="playTask"
        ></Task>
        <div class="task-input-block">
          <input
            type="text"
            class="task-input"
            placeholder="Add A New Mission"
            v-model="addTaskContent"
            @keyup.enter="addTask"
          />
          <button class="task-input-button" @click="addTask">
            <img src="./assets/plus.png" alt="plusBtn" />
          </button>
        </div>
      </div>
      <div class="task-complete-list">
        <div class="task-complete-header">
          <span>Recently Completed--</span>
          <span class="task-complete-more-button">More</span>
        </div>
        <Task
          v-for="task in doneTaskList"
          :content="task.content"
          :key="task.id"
          :id="task.id"
          :isShowPlayIcon="false"
          :status="task.status"
          :size="'small'"
        ></Task>
      </div>
    </div>
    <div class="right">
      <tomato :time="tomatoTime" :status="tomatoStatus"></tomato>
      <div class="time-counter-line"></div>
      <div class="action-block">
        <div v-if="!countId" class="play-button" @click="startCountDown"></div>
        <div v-else class="stop-button" @click="stopCountDown"></div>
        <div class="cancel-button" @click="clearCountDown"></div>
      </div>
    </div>
  </div>
</template>

<script>
import Tomato from "./components/Tomato.vue";
import Task from "./components/Task.vue";

const startTime = 1500000 //25mins
const coolDownTime = 300000 //5mins
// const startTime = 2000; //25mins
// const coolDownTime = 2000; //5mins
const taskList = [
  {
    content: "Do exercise",
    time: startTime,
    coolDownTime: coolDownTime,
    status: "undo",
    id: 0,
  },
  {
    content: "Have dinner",
    time: startTime,
    status: "undo",
    coolDownTime: coolDownTime,
    id: 1,
  },
  {
    content: "Read document",
    time: startTime,
    coolDownTime: coolDownTime,
    status: "done",
    id: 2,
  },
];
export default {
  name: "App",
  components: {
    Tomato,
    Task,
  },
  data() {
    return {
      taskList: taskList,
      count: 2,
      playedTask: {},
      addTaskContent: "",
      countId: "",
    };
  },
  computed: {
    unDoTaskList() {
      return this.taskList.filter((item) => item.status === "undo");
    },
    unDoTaskListUnplayed() {
      return this.unDoTaskList.filter((item) => item.id !== this.playedTask.id);
    },
    hasPlayedTask() {
      return Object.keys(this.playedTask).length;
    },
    doneTaskList() {
      return this.taskList.filter((item) => item.status === "done");
    },
    tomatoStatus() {
      return this.playedTask.time === startTime || !this.hasPlayedTask
        ? "normal"
        : this.playedTask.time === 0
        ? "coolDown"
        : "processing";
    },
    isCoolDown() {
      return this.tomatoStatus === "coolDown";
    },
    tomatoTime() {
      if (!this.hasPlayedTask) return 0;
      return this.isCoolDown
        ? this.playedTask.coolDownTime
        : this.playedTask.time;
    },
  },
  methods: {
    doneTask(id) {
      this.taskList.find((el) => el.id === id).status = "done";
      if (id === this.playedTask.id)
        this.playedTask = this.unDoTaskList.length ? this.unDoTaskList[0] : {};
    },
    addTask() {
      console.log("huiiii enter click");
      if (!this.addTaskContent) return;
      this.taskList.push({
        content: this.addTaskContent,
        status: "undo",
        time: startTime,
        id: ++this.count,
        coolDownTime: coolDownTime,
      });
      this.addTaskContent = "";
    },
    startCountDown() {
      if(!this.hasPlayedTask) return
      let remainSecond = this.tomatoTime;
      let _self = this;
      let countDown = setInterval(function () {
        remainSecond = remainSecond - 1000;
        _self.isCoolDown
          ? (_self.playedTask.coolDownTime = remainSecond)
          : (_self.playedTask.time = remainSecond);

        if (remainSecond < 1000) {
          if (_self.isCoolDown && _self.playedTask.coolDownTime !== 0)
            return (remainSecond = _self.playedTask.coolDownTime);
          clearInterval(countDown);
          _self.countId = "";
          _self.playedTask.status = "done";
          _self.playedTask = _self.unDoTaskList.length
            ? _self.unDoTaskList[0]
            : {};
          return;
        }
      }, 1000);
      this.countId = countDown;
      // let remainSecond = this.playedTask.time
      // let _self = this
      // let countDown = setInterval(function () {
      //   remainSecond = remainSecond - 1000
      //   _self.playedTask.time = remainSecond
      //   console.log(_self.playedTask.time)

      //   if (remainSecond < 1000) {
      //     console.log('hiiii')
      //     // mission.status = 'success'
      //     clearInterval(countDown)
      //     _self.countId = ''
      //     // this.stopCountDown(countDown)
      //     _self.playedTask.status = 'done'
      //     console.log(_self.unDoTaskList[0])
      //     _self.playedTask = _self.unDoTaskList.length ? _self.unDoTaskList[0] : {}
      //     return;
      //   }
      // }, 1000);
      // this.countId = countDown
    },
    stopCountDown() {
      clearInterval(this.countId);
      this.countId = "";
    },
    clearCountDown() {
      if(!this.hasPlayedTask) return
      this.stopCountDown();
      this.playedTask.time = startTime;
    },
    playTask(id) {
      this.stopCountDown();
      this.playedTask = this.taskList.find((item) => item.id === id);
    },
  },
  mounted() {
    this.playedTask = this.unDoTaskList[0];
  },
};
</script>

<style lang="scss">
body {
  margin: 0;
  box-sizing: border-box;
}
#app {
  width: 100vw;
  height: 100vh;
  background-color: $background-color;
  font-family: reross-rectangular, sans-serif;
  font-weight: 400;
  font-style: normal;
}
.main {
  padding: 50px 25px;
  // position: relative;
  display: flex;
  height: -webkit-fill-available;
}
.left {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.right {
  display: flex;
  flex-direction: column;
}
.chat {
  width: 100%;
  min-width: 200px;
}
.chat-bubble {
  background-color: white;
  width: 100%;
  height: 100px;
  border-radius: 65px;
  position: relative;
  padding: 10px 40px;
  display: flex;
  align-items: center;
  font-size: 45px;
}
.chat-bubble-tail {
  height: 50px;
  width: 50px;
  background: white;
  position: absolute;
  right: -10px;
  bottom: 2px;
  border-radius: 50%;
}
.chat-bubble-tail:after {
  height: 25px;
  width: 25px;
  background: white;
  position: absolute;
  right: -25px;
  bottom: 0;
  border-radius: 50%;
  content: "";
  display: block;
}
.chat-text {
  font-size: 30px;
  font-weight: 500;
}
.task-list {
  // margin-top: 75px;
  padding: 60px 45px;
  flex: 1;
}

.task-input-block {
  border: 3px solid $color-red-dark;
  border-radius: 5px;
  background: #ffffff 0% 0% no-repeat padding-box;
  height: 70px;
  display: flex;
  .task + & {
    margin-top: 40px;
  }
}
.task-input,
.task-input:focus,
.task-input:focus-visible:focus,
.task-input:focus-within:focus,
.task-input:active {
  flex: 1;
  padding: 0 15px !important;
  border: 0 !important;
  font-size: 30px;
  &:focus {
    padding: 0;
    border: 0;
  }
}
.task-input-button {
  background: #ffffff 0% 0% no-repeat padding-box;
  border: 0;
  border-left: 3px solid $color-red-dark;
  height: 100%;
  padding: 0;
  padding: 5px 10px;
}
.task-complete-list {
  background-color: $color-background-white;
  border-radius: 3px;
  padding: 15px 30px;
  .task {
    color: $color-gray;
  }
}
.task-complete-header {
  color: $color-gray;
  font-size: 20px;
  margin-bottom: 25px;
}
.task-complete-more-button {
  color: $color-gray-green;
  font-size: 25px;
  margin-left: auto;
  float: right;
}
.time-counter-line {
  border: 5px solid #ffffff;
  margin: 40px 10px;
}
.action-block {
  position: relative;
  justify-content: center;
  width: 100%;
  flex: 1;
}
.play-button {
  width: 100px;
  height: 100px;
  background: url("./assets/play.png");
  background-size: contain;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.stop-button {
  width: 100px;
  height: 100px;
  background: url("./assets/stop.png");
  background-size: contain;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.cancel-button {
  background: url("./assets/cancel.png");
  width: 40px;
  height: 40px;
  background-size: contain;
  position: absolute;
  left: 63%;
  top: 50%;
  transform: translateY(-50%);
}
</style>
