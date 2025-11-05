<script setup lang="ts">
import { ref } from "vue";
import Card from "primevue/card";
import FloatLabel from "primevue/floatlabel";
import InputText from "primevue/inputtext";
import InputMask from "primevue/inputmask";
import Dropdown from "primevue/dropdown";
import Calendar from "primevue/calendar";
import InputNumber from "primevue/inputnumber";
import Button from "primevue/button";

const form = ref({
  name: "",
  phone: "",
  email: "",
  gender: null,
  birthdate: null,
  salary: null,
});

const genders = [
  { label: "Laki-laki", value: "M" },
  { label: "Perempuan", value: "F" },
];

const onSubmit = () => {
  console.log("Form submitted:", form.value);
};
</script>

<template>
  <div class="p-6">
    <Card>
      <template #title>Formulir Pendaftaran</template>
      <template #content>
        <form
          @submit.prevent="onSubmit"
          class="grid grid-cols-1 md:grid-cols-2 gap-6"
        >
          <!-- Kolom Kiri -->
          <div class="flex flex-col space-y-4">
            <FloatLabel variant="on">
              <InputText id="name" v-model="form.name" class="w-full" />
              <label for="name">Nama Lengkap</label>
            </FloatLabel>

            <FloatLabel variant="on">
              <InputMask
                id="phone"
                v-model="form.phone"
                mask="(999) 999-9999"
                class="w-full"
              />
              <label for="phone">Nomor Telepon</label>
            </FloatLabel>

            <FloatLabel variant="on">
              <InputText id="email" v-model="form.email" class="w-full" />
              <label for="email">Email</label>
            </FloatLabel>
          </div>

          <!-- Kolom Kanan -->
          <div class="flex flex-col space-y-4">
            <FloatLabel variant="on">
              <Dropdown
                id="gender"
                v-model="form.gender"
                :options="genders"
                optionLabel="label"
                optionValue="value"
                class="w-full text-left"
              />
              <label for="gender">Jenis Kelamin</label>
            </FloatLabel>

            <FloatLabel variant="on">
              <Calendar
                id="birth"
                v-model="form.birthdate"
                showIcon
                dateFormat="dd/mm/yy"
                class="w-full"
              />
              <label for="birth">Tanggal Lahir</label>
            </FloatLabel>

            <FloatLabel variant="on">
              <InputNumber
                id="salary"
                v-model="form.salary"
                mode="currency"
                currency="IDR"
                locale="id-ID"
                class="w-full"
              />
              <label for="salary">Gaji (IDR)</label>
            </FloatLabel>
          </div>

          <!-- Tombol -->
          <div class="col-span-1 md:col-span-2 flex justify-end pt-4">
            <Button type="submit" label="Kirim" icon="pi pi-send" />
          </div>
        </form>
      </template>
    </Card>
  </div>
</template>

<style scoped>
/* Responsive improvement for small screens */
@media (max-width: 768px) {
  .p-float-label {
    width: 100%;
  }
}
</style>
