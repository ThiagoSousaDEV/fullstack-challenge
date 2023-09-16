<template>
  <Header />
  <Message />
  <Table />
</template>

<script>
import Header from "./components/Header.vue";
import Message from "./components/Message.vue";
import Table from "./components/Table.vue";

export default {
  name: "App",
  components: {
    Header,
    Table,
    Message,
  },
};
</script>

