<script setup lang="ts">
import TextField from "../field/TextField.vue";
import RadioButtonField from "../field/RadioButtonField.vue";
import NumberField from "../field/NumberField.vue";
import MaskField from "../field/MaskField.vue";
import useValidation from "../../utils/zod-validation";
import type { TrafoModel } from "../../types/trafo-type";
import { requiredNumber, requiredString } from "../../utils/zod-helper";
import { z } from "zod";

const form = defineModel<TrafoModel>({ required: true });

const validationSchema = z.object({
  name: requiredString("Name"),
  phase: requiredString("Phase"),
  brand: requiredString("Brand"),
  type: requiredString("Type"),
  voltage: requiredString("Voltage"),
  current: requiredString("Current"),
  capacity: requiredNumber("Capacity"),
  latitude: requiredNumber("Latitude"),
  longitude: requiredNumber("Longitude"),
});

const { validate, isValid, getError } = useValidation(validationSchema, form, {
  mode: "lazy",
});

const submit = async () => {
  await validate();
  return isValid.value;
};

defineExpose({ submit });
</script>

<template>
  <form class="mt-4 flex flex-col space-y-1" @submit.prevent="submit">
    <TextField v-model="form.name" label="Name" name="name" :error="getError" />
    <RadioButtonField
      v-model="form.phase"
      label="Phase"
      name="phase"
      :items="[
        { label: '1 Phase', value: '1 Phase' },
        { label: '3 Phase', value: '3 Phase' },
      ]"
      :error="getError"
    />
    <TextField
      v-model="form.brand"
      label="Brand"
      name="brand"
      :error="getError"
    />
    <TextField v-model="form.type" label="Type" name="type" :error="getError" />

    <label>Ratio Transformers</label>
    <div class="grid grid-cols-2 gap-4">
      <MaskField
        v-model="form.voltage"
        label="Voltage (VT)"
        name="voltage"
        mask="9:9"
        placeholder="1:1"
        :error="getError"
      />
      <MaskField
        v-model="form.current"
        label="Current (CT)"
        name="current"
        mask="9:9"
        placeholder="1:1"
        :error="getError"
      />
    </div>

    <NumberField
      v-model="form.capacity"
      label="Capacity"
      name="capacity"
      suffix=" KVA"
      :error="getError"
    />
    <NumberField
      v-model="form.latitude"
      label="Latitude"
      name="latitude"
      :maxDecimalDigits="6"
      :error="getError"
    />
    <NumberField
      v-model="form.longitude"
      label="Longitude"
      name="longitude"
      :maxDecimalDigits="6"
      :error="getError"
    />
  </form>
</template>
