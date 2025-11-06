<script setup lang="ts">
import FormTrafo from "./FormTrafo.vue";
import type { TrafoModel } from "../../types/trafo-type";
import { reactive, ref } from "vue";
import { Button, Dialog } from "primevue";
import { trafoService } from "../../service/trafo-service";
import { useAlert } from "../../utils/toast-helper";
import type { RowTrafoRes } from "../../api/trafo-api";

const props = defineProps<{ trafo: RowTrafoRes }>();
const emit = defineEmits<{ (e: "updated"): void }>();
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

function getDetailTrafo() {
  visible.value = true;
  form.id = props.trafo.id;
  form.name = props.trafo.name;
  form.phase = props.trafo.phasa;
  form.brand = props.trafo.brand;
  form.type = props.trafo.type;
  form.voltage = props.trafo.voltase.toString();
  form.current = props.trafo.current.toString();
  form.capacity = props.trafo.kapasitas;
  form.latitude = props.trafo.latitude;
  form.longitude = props.trafo.longitude;
}

function submitForm() {
  formElement.value?.submit().then((valid) => {
    if (!valid) return;
    trafoService.update(form.id, form).then(() => {
      visible.value = false;
      alert.success("Trafo updated successfully");
      emit("updated");
    });
  });
}
</script>

<template>
  <Button
    icon="pi pi-pen-to-square"
    severity="warn"
    rounded
    aria-label="Edit Trafo"
    @click="getDetailTrafo"
  />

  <Dialog
    v-model:visible="visible"
    modal
    header="Edit Trafo"
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
