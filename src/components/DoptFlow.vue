<script setup lang="ts">
import { Dopt } from '@dopt/javascript';
import { ref } from 'vue';

const { dopt } = defineProps<{ dopt: Dopt }>();

const flow = dopt.flow('mihai');

const flowState = ref(flow.state);
flow.subscribe(({ state }) => {
  flowState.value = state;
});

const modalBlock = dopt.block<['default']>('mihai.modal-block');
const modalBlockState = ref(modalBlock.state);
modalBlock.subscribe(({ state }) => {
  modalBlockState.value = state;
});

</script>

<template v-if="flow">
  <button
    v-if="modalBlockState.active"
    @click="modalBlock.transition('complete')"
  >
  {{ modalBlock.sid}}
  </button>
</template>
