<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <button class="btn btnPrimary" @click="modalValidate = true">
            Show modal with validation
          </button>
          <ModalValidate
            v-show="modalValidate"
            @close="modalValidate = false"
          ></ModalValidate>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import ModalValidate from "@/components/ModalValidate";

export default {
  components: { ModalValidate },
  data() {
    return {
      modalValidate: false,
    };
  },
  computed: {},
  methods: {},
};
</script>

<style lang="scss" scoped>
.wrapper {
  align-items: center;
  justify-content: center;

  &-content {
    min-height: auto;
  }
}
</style>
