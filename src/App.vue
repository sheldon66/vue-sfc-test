<template>
  <div class="progress">
    <!-- <div class="progress-bar" @click="dotMove" @mousemove="dotDrag"> -->
    <div class="progress-bar" @mousemove="dotDrag" @click="dotMove">
      <div class="unfilled"></div>
      <div class="filled" ref="filled" :style="{ width: dot.width + 'px' }">
        <div class="dot" @mousedown="dotDown" @mouseup="dotUp"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "prograss",
  data: function () {
    return {
      dot: { moveable: false, startX: 0, shiftX: 0, width: 0, tempWidth: 0 },
    };
  },
  methods: {
    dotMove: function (e) {
      e.preventDefault();
      this.dot.width = e.clientX - this.dot.startX + this.dot.tempWidth;
    },
    dotDown: function (e) {
      this.dot.moveable = true;
      this.dot.startX = e.clientX;
      this.dot.tempWidth = this.dot.width;
    },
    dotUp: function (e) {
      this.dot.moveable = false;
      this.dot.startX = e.clientX;
    },
    dotDrag: function (e) {
      if (!this.dot.moveable) {
        return false;
      }
      this.dotMove(e);
    },
    bindEvents: function () {
      document.addEventListener("mousemove", this.dotDrag);
      document.addEventListener("mouseup", this.dotUp);
    },
    unbindEvents: function () {
      document.addEventListener("mousemove", this.dotDrag);
      document.addEventListener("mouseup", this.dotUp);
    },
  },
  mounted: function () {
    this.$nextTick(() => {
      this.bindEvents();
    });
  },
  beforeDestroy() {
    this.unbindEvents();
  },
};
</script>
<style lang="scss">
.progress {
  margin: 20px;
}
.progress-bar {
  height: 50px;
  width: 100%;
  position: relative;
}
.filled {
  background-color: red;
  height: 20px;
  position: absolute;
  left: 0;
}
.unfilled {
  background-color: blue;
  height: 20px;
  width: 100%;
  position: absolute;
  left: 0;
}
.dot {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background-color: black;
  position: absolute;
  right: -10px;
}
</style>
