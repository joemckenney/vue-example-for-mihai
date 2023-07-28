<script setup lang="ts">
  import { ref } from 'vue';

  import { DoptApiClient as UsersApiClient } from '@dopt/users-javascript-browser-client';
  import { Dopt as DoptJavaScriptClient } from '@dopt/javascript';

  import DoptFlow from './components/DoptFlow.vue';

  let dopt: DoptJavaScriptClient;

  const doptReady = ref(false);
  const flowReady = ref(false);

  (async function () {

    const usersClient = new UsersApiClient({
      apiKey: "YOUR_USERS_API_KEY"
    });

    const userId = 'test-user'; // a stable identifier for your user

    await usersClient.users.identifyUser({
      identifier: userId,
      properties: { city: 'Oakland' },
    });

    dopt = new DoptJavaScriptClient({
      apiKey: "YOUR_BLOCKS_API_KEY",
      userId,
      flowVersions: {
        'mihai': 0,
      }
    });

    await dopt.flow('mihai').initialized();

    flowReady.value = true;
    doptReady.value = true;
  })();
</script>

<template>
  <div v-if="!doptReady" id="dopt-pending" />
  <div v-if="!flowReady" id="flow-pending" />
  <div v-if="flowReady" id="flow-succeeded" />
  <template v-if="doptReady">
    <DoptFlow :dopt="dopt" />
  </template>
</template>

