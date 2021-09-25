<template>
  <transition name="my-toast-smooth">
    <div
      :class="[
        'my-toast-container',
        'my-toast-' + type,
        'my-toast-position-' + position,
      ]"
      v-if="show"
      :style="{ width: width + 'px', height: height + 'px' }"
    >
      <a class="my-toast-close-btn" @click.prevent="closeToast">
        <span aria-hidden="true"> &times; </span>
      </a>
      <div :class="['my-toast-icon', icon]"></div>
      <div class="my-toast-content">
        <div class="my-toast-title">{{ title }}</div>
        <div class="my-toast-message">{{ message }}</div>
      </div>
    </div>
  </transition>
</template>
<script>
export default {
  name: "my-toast",
  props: {
    // toast 位置
    position: {
      type: String,
      default: "topCenter",
      validator(value) {
        return (
          [
            "topCenter",
            "topLeft",
            "topRight",
            "bottomLeft",
            "bottomRight",
          ].indexOf(value) !== -1
        );
      },
    },
    // 提示消息
    message: {
      type: String,
      default: "",
    },
    // 提示类型
    type: {
      type: String,
      default: "info",
      validator(value) {
        return ["info", "success", "error", "warning"].indexOf(value) !== -1;
      },
    },
    // 回调函数
    callback: {
      type: Function,
    },
    // 图标
    icon: {
      type: String,
      default: "",
    },
    // 标题
    title: {
      type: String,
      default: "",
    },
    // 关闭时间
    closeTime: {
      type: Number,
    },
    width: {
      type: Number,
      default: 300,
    },
    height: {
      type: Number,
      default: 80,
    },
    autoClose: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      show: false,
    };
  },
  methods: {
    close() {
      if (this.autoClose) {
        setTimeout(() => {
          this.show = false;
          this.callback && this.callback();
        }, this.closeTime * 1000);
      }
    },
    closeToast() {
      this.show = false;
      this.callback && this.callback();
    },
  },
  mounted() {
    console.log(this.$props);
    this.show = true;
    this.close();
  },
};
</script>
<style scoped lang='scss'>
@font-face {
  font-family: "my-toast"; /* project id 1440887 */
  src: url("//at.alicdn.com/t/font_1440887_nejwkx8cakj.eot");
  src: url("//at.alicdn.com/t/font_1440887_nejwkx8cakj.eot?#iefix")
      format("embedded-opentype"),
    url("//at.alicdn.com/t/font_1440887_nejwkx8cakj.woff2") format("woff2"),
    url("//at.alicdn.com/t/font_1440887_nejwkx8cakj.woff") format("woff"),
    url("//at.alicdn.com/t/font_1440887_nejwkx8cakj.ttf") format("truetype"),
    url("//at.alicdn.com/t/font_1440887_nejwkx8cakj.svg#mbs-toast")
      format("svg");
}
.my-toast-icon {
  font-family: "my-toast" !important;
  position: absolute;
  font-size: 40px;
  top: 7px;
  left: 15px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.my-toast-icon-success:before {
  content: "\e651";
}
.my-toast-icon-error:before {
  content: "\e652";
}
.my-toast-icon-info:before {
  content: "\e654";
}
.my-toast-icon-warning:before {
  content: "\e655";
}

.my-toast-smooth-enter,
.my-toast-smooth-leave-to {
  will-change: transform, opacity;
  -webkit-transform: scale(0.5);
  transform: scale(0.5);
  opacity: 0;
  -webkit-transition: transform 200ms ease, opacity 200ms ease;
  transition: transform 200ms ease, opacity 200ms ease;
}

.my-toast-smooth-enter-to,
.my-toast-smooth-leave {
  will-change: transform, opacity;
  opacity: 1;
  -webkit-transition: transform 250ms ease, opacity 250ms ease;
  transition: transform 250ms ease, opacity 250ms ease;
}

.my-toast-container {
  border-radius: 3px;
  box-shadow: 0 1px 8px rgba(0, 0, 0, 0.5);
  color: #fff;
  position: fixed;
  z-index: 2000;

  my-toast-close-btn {
    position: absolute;
    right: 5px;
    top: 0;
    -webkit-appearance: none;
    cursor: pointer;
    outline: none;
    font-size: 18px;
    line-height: 100%;
    &:hover {
      color: black !important;
    }
  }
  &.my-toast-info {
    color: #000;
    .my-toast-close-btn {
      color: #999;
    }
  }
  &.mbs-toast-success {
    background: #2CBE4E;
    .mbs-toast-close-btn {
      color: #fff;
    }
  }
  &.mbs-toast-warning {
    background: #F5CF87;
    color: #000;
    .mbs-toast-close-btn {
      color: #999;
    }
  }
  &.mbs-toast-error {
    background: #FF5252;
    .mbs-toast-close-btn {
      color: #fff;
    }
  }
  /* ['topCnter','topLeft','topRight','bottomLeft','bottomRight'] */
  &.mbs-toast-position-topCenter {
    top: 20px;
    left: 50%;
    transform: translateX(-50%);
  }
  &.mbs-toast-position-topLeft {
    top: 20px;
    left: 20px;
  }
  &.mbs-toast-position-topRight {
    top: 20px;
    right: 20px;
  }
  &.mbs-toast-position-bottomLeft {
    bottom: 20px;
    left: 20px;
  }
  &.mbs-toast-position-bottomRight {
    bottom: 20px;
    right: 20px;
  }
  .mbs-toast-content {
    padding-left: 70px;
    padding-top: 12px;
    padding-right: 30px;
    .mbs-toast-title {
      font-size: 16px;
      font-weight: bold;
    }
    .mbs-toast-message {
      letter-spacing: 1px;
      font-size: 14px;
    }
  }
}
</style>
