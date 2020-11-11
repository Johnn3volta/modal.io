<template>
  <transition name="modal">
    <div class="modal" v-if="modalShow">
      <div @click="close" class="modal-background"></div>
      <div class="modal-content">
        <span @click="close" class="modal-close">
          <svg version="1" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path d="M13 12l5-5-1-1-5 5-5-5-1 1 5 5-5 5 1 1 5-5 5 5 1-1z"></path>
          </svg>
        </span>
        <slot></slot>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Modal',
  props: {
    modalShow: {
      type: Boolean,
    }
  },
  methods: {
    close() {
      this.$emit('close');
    }
  }
};
</script>

<style lang="scss" scoped>
.modal,
.modal-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  z-index: 1000;
}

.modal {
  display: flex;
  align-items: center;
  justify-content: center;
  transition: opacity .3s ease, transform .3s ease;

  &-background {
    background-color: rgba(10, 10, 10, .8);
    z-index: 40;
  }

  &-content {
    z-index: 41;
    padding: 0;
    background-color: #e9e9e9;
    cursor: auto !important;
    position: relative;
    height: 80vh;
    overflow: hidden;
  }

  &-close {
    display: block;
    width: 30px;
    height: 30px;
    position: absolute;
    right: 0;
    z-index: 42;
    cursor: pointer;
    transition: all .3s ease;
    opacity: .7;

    &:hover {
      opacity: 1;
    }
  }
}

.modal-enter-active {
  opacity: 1;
}

.modal-enter {
  opacity: 0;
  transform: scale(1.5, 1.5);
}

.modal-leave-active {
  opacity: 0;
  transform: scale(1.5, 1.5);
}

.modal-leave {
  opacity: 1;
}
</style>