<template>
  <hr />
  <h5>
    Total tasks: <span class="primary">{{ todos }} </span> | Completed tasks:
    <span class="primary">{{ todosDone }}</span>
  </h5>
  <h5
    @click="$emit('deleteAlltodos')"
    class="del-btn"
  >
    Delete all tasks
  </h5>
</template>
<script>
export default {
  props: ['todos', 'todosDone'],
  emits: ['deleteAlltodos'],
}
</script>
<style></style>
