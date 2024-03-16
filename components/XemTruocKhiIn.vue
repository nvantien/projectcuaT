<script lang="ts" setup>
import { VueToPrint } from "vue-to-print";
import { reactive, ref, type PropType } from "vue";

interface HoSoKhamBenh {
  tenBenhNhan: String,
  maYte: String,
}

const props = defineProps({
  hoSoKhamBenh: {
    type: Object as PropType<HoSoKhamBenh>,
    required: true,
  }
});

const componentRef = ref();
const state = reactive({
  isLoading: false,
  text: "old boring text"
});

const handleAfterPrint = () => {
  console.log("`onAfterPrint` called"); // tslint:disable-line no-console
};

const handleBeforePrint = () => {
  console.log("`onBeforePrint` called"); // tslint:disable-line no-console
};

const handleOnBeforeGetContent = () => {
  console.log("`onBeforeGetContent` called"); // tslint:disable-line no-console
  state.text = "Loading new text...";
  state.isLoading = true;

  return new Promise<void>((resolve) => {
    setTimeout(() => {
      state.text = "New, Updated Text!";
      state.isLoading = false;

      resolve();
    }, 2000);
  });
};

const getComponentToPrint = () => {
  return componentRef.value;
};
</script>
<style>
.print-table {
  border-collapse: collapse;
}
</style>
<template>
  <VCard>
    <VCardTitle>In ho so benh nhan</VCardTitle>
    <VCardText>
      <div ref="componentRef">
        <component-to-print :text="state.text" />
        <VImg  src="/hosokhambenh-header.jpg" />
        <table border="1" class="w-100 print-table">
          <tr>
            <td>Ten benh nhan</td>
            <td>{{ props.hoSoKhamBenh.tenBenhNhan }}</td>
            <td>Mã Y Tế</td>
            <td>{{ props.hoSoKhamBenh.maYTe }}</td>
          </tr>
        </table>
        <VImg  src="/hosokhambenh-footer.jpg" />
      </div>
    </VCardText>
    <VCardActions>
      <vue-to-print :content="getComponentToPrint" document-title="AwesomeFileName" :on-after-print="handleAfterPrint"
        :on-before-get-content="handleOnBeforeGetContent" :on-before-print="handleBeforePrint" remove-after-print>
        <p v-if="state.isLoading" class="indicator">onBeforeGetContent: Loading...</p>
        <template #trigger>
          <VBtn prepend-icon="mdi-printer">In</VBtn>
        </template>
      </vue-to-print>
    </VCardActions>
  </VCard>
</template>