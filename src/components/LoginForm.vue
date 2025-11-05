<script setup lang="ts">
import { reactive } from "vue";
import Card from "primevue/card";
import FloatLabel from "primevue/floatlabel";
import InputText from "primevue/inputtext";
import InputGroup from "primevue/inputgroup";
import InputGroupAddon from "primevue/inputgroupaddon";
import Button from "primevue/button";

type LoginField = {
  username: string;
  password: string;
};

const form = reactive<LoginField>({
  username: "",
  password: "",
});

const onSubmit = async () => {
  const formData = new URLSearchParams();
  formData.append("username", form.username);
  formData.append("password", form.password);

  const response = await fetch("http://127.0.0.1:8000/login", {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
    },
    body: formData,
  });

  const data = await response.json();
  console.log(data);
};
</script>

<template>
  <div class="flex justify-center items-center h-screen">
    <Card class="w-1/3 shadow-2xl border border-gray-200">
      <template #title>Login</template>
      <template #content>
        <form @submit.prevent="onSubmit">
          <div class="flex flex-col space-y-4">
            <FloatLabel variant="on">
              <InputText id="username" v-model="form.username" class="w-full" />
              <label for="username">Username</label>
            </FloatLabel>

            <InputGroup>
              <FloatLabel variant="on">
                <InputText
                  id="password"
                  v-model="form.password"
                  class="w-full rounded-2xl"
                />
                <label for="password">Password</label>
              </FloatLabel>
              <InputGroupAddon>
                <i class="pi pi-eye"></i>
              </InputGroupAddon>
            </InputGroup>
          </div>

          <div class="block pt-6">
            <Button type="submit" class="w-full" rounded label="LOGIN" />
          </div>
        </form>
      </template>
    </Card>
  </div>
</template>
