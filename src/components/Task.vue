<template>
  <div class="task" :class="taskSize">
    <div v-if="status === 'undo'">
      <input
        type="checkbox"
        :id="'task_' + id"
        name="checkbox1"
        v-model="checked"
      />
      <label :for="'task_' + id">
        <span class="checkbox"></span>
      </label>
    </div>
    <div v-else>
      <img src="../assets/checked.png" alt="checked" class="checked-icon" />
    </div>
    <span class="task-text">{{ content }}</span>
    <img
      src="../assets/taskPlay.png"
      alt="playtask"
      class="play-icon"
      v-if="isShowPlayIcon"
      @click="playTask"
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
    status: {
      type: String,
      default: "undo",
    },
    size: {
      type: String,
      default: "medium",
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
  computed: {
    taskSize() {
      return `task-${this.size}`;
    },
  },
  methods: {
    playTask(){
      this.$emit("playTask", this.id)
    }
  },
};
</script>

<style lang="scss">
.task {
  display: flex;
  align-items: center;
}
.task-big {
  .task-text {
    font-size: 45px;
  }
}
.task-small {
  .task-text {
    font-size: 25px;
  }
}
.task-medium {
  .task-text {
    font-size: 30px;
  }
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
.checked-icon {
  width: 30px;
  height: 30px;
  margin-right: 8px;
}
</style>