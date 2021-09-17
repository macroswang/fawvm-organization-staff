<template>
  <div v-if="_visible">
    <div class="modal-container slide-fade" :style="`z-index: ${zIndex}`">
      <div class="modal-wrap">
        <div
          v-if="mask"
          class="modal-mask"
          :style="`z-index: ${zIndex};${maskStyle}`"
          @click.stop="handleCancel"
        />
        <div class="modal-box" :style="`width: ${width};z-index: ${zIndex};${contentStyle};`">
          <div class="modal-content">
            <!-- 关闭按钮 -->
            <div class="close-btn" @click="handleCancel">
              <div class="close-line" />
              <div class="close-line" />
            </div>

            <!-- header -->
            <div v-if="title" class="modal-header">
              <h2>{{ title }}</h2>
            </div>

            <slot />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  props: {
    visible: {
      type: Boolean,
      default: false
    },
    width: {
      type: String,
      default: '520px'
    },
    contentStyle: {
      type: String,
      default: ''
    },
    zIndex: {
      type: Number,
      default: 999
    },
    mask: {
      type: Boolean,
      default: true
    },
    maskStyle: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      loading: false,
      body: null
    }
  },
  computed: {
    _visible: {
      get() {
        return this.visible
      },
      set(val) {
        this.$emit('update:visible', val)
      }
    }
  },
  watch: {
    // 切换路由关闭弹窗
    $route() {
      this._visible = false
    }
  },
  mounted() {
    this.$nextTick(() => {
      const body = document.querySelector('body')
      if (body.append) {
        body.append(this.$el)
      } else {
        body.appendChild(this.$el)
      }
    })
  },
  // 不是缓存状态下，切换路由销毁组件
  destroyed() {
    try {
      const body = document.querySelector('body')
      body.removeChild(this.$el)
    } catch (error) {
      console.log(error)
    }
  },
  methods: {
    handleCancel() {
      this._visible = false
    }
  }
}
</script>

<style lang="less">
/* 动画 */
@keyframes animation {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.slide-fade {
  animation-name: animation;
  animation-duration: 0.3s;
}

.modal-container {
  position: relative;
  z-index: 999;

  .modal-mask {
    background: rgba(0, 0, 0, 0.4);
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  }
  .modal-wrap {
    position: fixed;
    right: 0;
    left: 0;
    top: 0;
    bottom: 0;
    overflow: auto;
    outline: 0;

    .modal-box {
      position: relative;
      top: 100px;
      margin: 0 auto;
      .modal-content {
        position: relative;
        background-color: #fff;
        padding: 20px 20px 10px 20px;
        border-radius: 5px;
        pointer-events: auto;

        .modal-header {
          border-bottom: 1px solid #e2e2e2;
          height: 40px;
          line-height: 28px;
          margin-bottom: 10px;
        }

        .close-btn {
          width: 25px;
          height: 25px;
          border-radius: 50%;
          background-color: #fff;
          position: absolute;
          right: -12px;
          top: -12px;
          display: flex;
          justify-content: center;
          align-items: center;
          cursor: pointer;
        }

        .close-line {
          width: 1px;
          height: 10px;
          background-color: #dbdbdb;
          transform-origin: center;
        }

        .close-line:nth-of-type(1) {
          transform: rotateZ(50deg);
        }

        .close-line:nth-of-type(2) {
          transform: rotateZ(-50deg);
        }
      }
    }
  }
}
</style>
