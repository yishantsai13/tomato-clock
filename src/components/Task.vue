<template>
  <div class="task">
    <input
      type="checkbox"
      :id="'task_' + id"
      name="checkbox1"
      v-model="checked"
    />
    <label :for="'task_' + id">
      <span class="checkbox"></span>
    </label>
    <span class="task-text">{{ content }}</span>
    <img
      src="../assets/taskPlay.png"
      alt=""
      class="play-icon"
      v-if="isShowPlayIcon"
    />
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    content: {
      type: String,
      required: true,
    },
    isShowPlayIcon: {
      type: Boolean,
      default: true,
    },
    id: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      checked: false,
    };
  },
  watch: {
    checked(val) {
      if (val) this.$emit("taskDone", this.id);
    },
  },
};
</script>

<style lang="scss">
.task {
  display: flex;
  align-items: center;
}
input[type="checkbox"] {
  display: none;
}
input[type="checkbox"] + label .checkbox {
  display: inline-block;
  cursor: pointer;
  border: 5px solid $color-red-dark;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  margin-right: 8px;
}
// input[type="checkbox"]:checked + label span {
//   display:inline-block;
//   // border: 5px solid black;
//   border-radius: 50%;
//   width: 30px;
//   height: 30px;
//   background:url("./assets/checked.png") no-repeat left top;
// }
.check-box {
  display: inline-block;
  border: 5px solid $color-red-dark;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  vertical-align: bottom;
}
.task-text {
  flex: 1;
  font-size: 30px;
  font-weight: 500;
}
.check-box + span {
  margin-left: 8px;
}
.check-box-big {
  width: 50px;
  height: 50px;
}
.task + .task {
  margin-top: 40px;
}
.play-icon {
  width: 35px;
  height: 35px;
  float: right;
}
</style>