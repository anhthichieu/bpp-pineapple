<template>
  <v-container>
    <div id="app-2">
      <span :title="message">
        Hover your mouse over me for a few seconds to see my dynamically bound
        title!
      </span>
    </div>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',

  data: () => ({
    message: 'You loaded this page on ' + new Date().toLocaleString(),
  }),
};
</script>
