<template>
  <div v-if="visible" class="popup-overlay">
    <div class="popup">
      <h2>Ви впевнені, що хочете видалити картку?</h2>
      <div class="btn_container">
        <Button @click="confirmAction('confirm')"> Так</Button>
        <Button @click="confirmAction('cancel')"> Ні </Button>
      </div>
    </div>
  </div>
</template>

<script>
import Button from "./ui/Button.vue";

export default {
  props: ["visible"],
  methods: {
    confirmAction(action) {
      this.$emit("confirm-action", action);
    },
  },
  components: {
    Button,
  },
};
</script>
    <style scoped>
.btn_container {
  display: flex;
  justify-content: center;
}
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  min-width: 400px;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup {
  /* min-width: 600px; */
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
}
</style>