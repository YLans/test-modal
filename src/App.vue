<template>
  <div class="main__container">
    <p class="main__info">{{ pageData }}</p>
    <button v-if="showModalBtn" class="btn main__show-modal" @click="getConfirm">Try again</button>
    <ModalMenu ref="modalConfirmation" />
  </div>
</template>

<script>
import { ref } from 'vue';

import ModalMenu from '@/components/ModalMenu.vue';

export default {
  name: 'App',
  components: {
    ModalMenu,
  },
  methods: {
    async getConfirm() {
      const modalPropmise = await this.$refs.modalConfirmation.show();
      if (modalPropmise) {
        this.pageData = `Your email is ${modalPropmise}`;
        this.showModalBtn = false;
      } else {
        this.pageData = 'You did not enter your email in modal window';
        this.showModalBtn = true;
      }
    },
  },
  mounted() {
    this.getConfirm();
  },
  setup() {
    const pageData = ref('');
    const showModalBtn = ref(false);

    return {
      pageData,
      showModalBtn,
    };
  },
};
</script>

<style lang="scss">
* {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
}
body {
  padding: 0;
  margin: 0;
  &.static {
    overflow: hidden;
  }
}

.btn {
  border: none;
  outline: none;
  background: transparent;
  transition: .3s;
  cursor: pointer;
  &:hover, &:active, &:focus {
    opacity: 0.7;
  }
  &:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}

.main {
  margin: 60px 0;
  &__container {
    max-width: 1310px;
    padding: 0 15px;
  }
  &__show-modal {
    padding: 0;
    border-bottom: 1px solid;
    &:hover, &:active, &:focus {
      border-bottom-color: transparent;
    }
  }
}
</style>
