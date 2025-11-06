<script setup lang="ts">
import { reactive, ref } from "vue";
import { Button, Dialog } from "primevue";
import { trafoService } from "../../service/trafo-service";
import { useAlert } from "../../utils/toast-helper";
import type { TrafoModel } from "../../types/trafo-type";
import FormTrafo from "./FormTrafo.vue";

const emit = defineEmits<{ (e: "submited"): void }>();
const formElement = ref<InstanceType<typeof FormTrafo>>();
const alert = useAlert();

const visible = ref(false);
const form = reactive<TrafoModel>({
  id: 0,
  name: "",
  phase: "",
  brand: "",
  type: "",
  voltage: "",
  current: "",
  capacity: null,
  latitude: null,
  longitude: null,
});

function submitForm() {
  formElement.value?.submit().then((valid) => {
    if (!valid) return;
    trafoService
      .save(form)
      .then(() => {
        visible.value = false;
        alert.success("Trafo saved successfully");
        emit("submited");
      })
      .catch((e) => alert.error(e));
  });
}
</script>

<template>
  <Button label="Add New Trafo" icon="pi pi-plus" @click="visible = true" />

  <Dialog
    v-model:visible="visible"
    modal
    header="Add New Trafo"
    :style="{ width: '35rem' }"
  >
    <FormTrafo ref="formElement" v-model="form" />

    <template #footer>
      <div class="pt-2 flex justify-end gap-2">
        <Button label="Cancel" severity="secondary" @click="visible = false" />
        <Button label="Save" @click="submitForm" />
      </div>
    </template>
  </Dialog>
</template>
