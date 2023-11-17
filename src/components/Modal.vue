<template>
  <div v-if="visible">
    <div class="popup-overlay">
      <div class="popup">
        <h2>Для додавання, видаліть місто (максимум 5 міст)</h2>
        <Button @click="closeModal">Закрити</Button>
      </div>
    </div>
  </div>
</template>
  
  <script>
import Button from "./ui/Button.vue";
export default {
  props: ["visible"],
  methods: {
    closeModal() {
      this.$emit("close-modal");
    },
  },
  components: {
    Button,
  },
};
</script>
  <style scoped>
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