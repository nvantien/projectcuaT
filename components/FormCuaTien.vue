<script setup lang="ts">
const name = ref('');
const nameRules = [
  (v: any) => !!v || 'Nhập tên',
  (v: string | any[]) => (v && v.length > 3 && v.length < 200) || 'Tên phải trên 3 ký tự va duoi 200 ky tu',
];

const hoSoKhamBenh = ref({
  tenBenhNhan: name.value,
  maYTe: codeid.value
});

const printPreview = ref({
  show: false
})

function print() {
  printPreview.value.show = true;
  hoSoKhamBenh.value.tenBenhNhan = name.value;
  hoSoKhamBenh.value.maYTe = codeid.value;
};

function reset() {
  name.value = '';
};
</script>
<template>
  <VSheet class="mx-auto" width="80%">
    <VForm ref="form">
      <VTextField v-model="codeid" :counter="200" :rules="nameRules" label="Mã Y Tế" required></VTextField>
      <VTextField v-model="name" :counter="200" :rules="nameRules" label="Tên" required></VTextField>

      <div class="d-flex flex-column">
        <VBtn class="mt-4" color="success" block @click="print">
          In
        </VBtn>
        <VBtn class="mt-4" color="error" block @click="reset">
          Reset
        </VBtn>
      </div>
    </VForm>
  </VSheet>
  <VDialog v-model="printPreview.show">
    <XemTruocKhiIn :ho-so-kham-benh="hoSoKhamBenh" />
  </VDialog>
</template>