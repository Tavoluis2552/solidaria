<template>
  <Dialog v-model:visible="showModal" header="Confirmar Eliminación" :modal="true">
    <div class="p-4">
      <p>¿Estás seguro de que deseas eliminar la categoría <strong>{{ category.name }}</strong>?</p>
    </div>
    <template #footer>
      <Button label="Cancelar" class="p-button-text" @click="closeModal" />
      <Button label="Eliminar" class="p-button-danger" @click="deleteCategory" />
    </template>
  </Dialog>
</template>

<script setup>
import { ref, watch, defineEmits } from "vue";
import Dialog from "primevue/dialog";
import Button from "primevue/button";
import axios from "axios";

const emit = defineEmits(["delete", "refreshList", "closeDeleteModal"]);

const props = defineProps({
  category: Object,
  visible: Boolean,
});

const showModal = ref(props.visible);

watch(() => props.visible, (val) => {
  showModal.value = val;
});

function closeModal() {
  showModal.value = false;
  emit("closeDeleteModal");
}

async function deleteCategory() {
  try {
    emit("delete", props.category.id);
    emit("refreshList");
    closeModal();
  } catch (error) {
    console.error("Error al eliminar la categoría:", error);
  }
}

</script>
