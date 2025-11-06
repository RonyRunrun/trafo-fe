<script setup lang="ts">
import Header from "../components/Header.vue";
import CreateTrafo from "../components/trafo/CreateTrafo.vue";
import UpdateTrafo from "../components/trafo/UpdateTrafo.vue";
import type { RowTrafoRes } from "../api/trafo-api";
import { Button, Card, Toolbar, DataTable, Column } from "primevue";
import { onMounted, ref } from "vue";
import { trafoService } from "../service/trafo-service";

const products = ref<RowTrafoRes[]>([]);

async function getDataTable() {
  const result = await trafoService.findAll({
    q: null,
    page: 0,
    size: 10,
  });
  products.value = result;
}

onMounted(() => {
  getDataTable();
});
</script>

<template>
  <Header title="System Management Gardu Distribusi PT PLN (Persero)" />

  <Toolbar class="rounded-2xl! my-4 mx-4">
    <template #start>
      <div class="flex items-center gap-2"></div>
    </template>

    <template #end>
      <div class="flex items-center gap-2">
        <CreateTrafo @submited="getDataTable" />
      </div>
    </template>
  </Toolbar>

  <Card class="mx-4 border border-gray-200">
    <template #content>
      <DataTable :value="products" tableStyle="min-width: 50rem">
        <template #empty>
          <div class="flex items-center justify-center h-42">
            <div class="text-center">
              <i class="pi pi-database text-gray-500"></i>
              <div class="text-gray-500">No Data</div>
            </div>
          </div>
        </template>

        <Column field="" header="No">
          <template #body="{ index }"> {{ index + 1 }} </template>
        </Column>
        <Column field="name" header="Trafo Name"></Column>
        <Column field="kapasitas" header="Capacity"></Column>
        <Column field="phasa" header="Phase"></Column>
        <Column field="action" header="Action">
          <template #body="{ data }">
            <div class="flex items-center gap-2">
              <UpdateTrafo :trafo="data" @updated="getDataTable" />
              <Button
                icon="pi pi-calculator"
                severity="success"
                rounded
                aria-label="Detail"
              />
            </div>
          </template>
        </Column>
      </DataTable>
    </template>
  </Card>
</template>
