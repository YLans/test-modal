<template>
  <div class="main__modal modal" v-show="showModal">
    <div class="modal__conatiner">
      <h2 class="modal__title">Enter your Email</h2>
      <button class="btn modal__btn-close" @click="close">&#10008;</button>
      <form class="modal__form" @submit="onSubmitHandler">
        <label for="email-field" class="modal__form__field">
          <input
            type="email"
            name="email-field"
            class="modal__form__field-input"
            placeholder="example@gmail.com"
            @input="onInputHandler"
          />
        </label>
        <button class="btn modal__form__btn" :disabled="btnDisabled">apply</button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'ModalMenu',
  methods: {
    show() {
      this.toggleModalVisibility();

      let resolve;
      let reject;

      const modalPromise = new Promise((success, fail) => {
        resolve = success;
        reject = fail;
      });

      this.modalOptions = { resolve, reject };

      return modalPromise;
    },
    onSubmitHandler(evt) {
      evt.preventDefault();
      const targetValue = evt.target[0].value;
      if (targetValue) {
        this.modalOptions.resolve(targetValue);
      }

      this.toggleModalVisibility();
    },
    toggleModalVisibility() {
      this.showModal = !this.showModal;
      document.body.classList.toggle('static');
    },
    close(evt) {
      evt.preventDefault();
      this.modalOptions.resolve(false);

      this.toggleModalVisibility();
    },
    onInputHandler(evt) {
      if (evt.target.value.length > 5) {
        this.btnDisabled = false;
      }
    },
  },
  setup() {
    const modalOptions = ref();
    const showModal = ref(false);
    const btnDisabled = ref(true);

    return {
      modalOptions,
      showModal,
      btnDisabled,
    };
  },
};
</script>

<style lang="scss">
.main__modal {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 30px;
  background: rgba(#000000, .7);
  .modal {
    &__conatiner {
      position: relative;
      width: 500px;
      padding: 30px;
      border-radius: 8px;
      background: #FFF;
    }
    &__title {
      margin-top: 0;
    }
    &__btn-close {
      position: absolute;
      top: 15px;
      right: 15px;
      border: none;
      outline: none;
    }
    &__form {
      display: flex;
      &__field-input, &__btn {
        padding: 10px 15px;
      }
      &__field-input {
        display: block;
        height: 100%;
        border: 2px solid rgba(#5d5759, 0.5);
        border-right-color: transparent;
        border-radius: 8px 0 0 8px;
        outline: none;
        box-sizing: border-box;
      }
      &__btn {
        margin-left: -2px;
        border-radius: 0 8px 8px 0;
        background: #5d5759;
        color: #fffbff;
        text-transform: uppercase;
      }
    }
  }
}
</style>
