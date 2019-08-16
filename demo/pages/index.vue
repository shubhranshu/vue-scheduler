<template>
  <v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <scheduler-demo />
    </v-flex>
  </v-layout>
</template>

<script>
import SchedulerDemo from '~/components/SchedulerDemo';
export default {
  components: {
    SchedulerDemo,
  },
};
</script>
