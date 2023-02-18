<template>
  <li
    :class="['list-item', { done: todo.done, edit: todo.edit }]"
    @click.stop.prevent="$emit('done', index)"
  >
    <div class="list-item__title">{{ index + 1 }}. {{ todo.task }}</div>
    <div :class="['list-item__icons', { none: todo.edit }]">
      <box-icon
        @click.stop.prevent="$emit('edit', index)"
        name="message-square-edit"
        animation="tada-hover"
        size="sm"
      ></box-icon>

      <box-icon
        @click.stop.prevent="$emit('remove', index)"
        name="message-square-x"
        animation="tada-hover"
        size="sm"
      ></box-icon>
    </div>
  </li>
  <!-- <li :class="['list-item', { done: todo.done }]">{{ todo.task }}</li> -->
</template>
<script>
export default {
  props: ['index', 'todo'],
  emits: ['remove', 'done', 'edit'],

  // computed: {
  //   todosEdit() {
  //     // console.log({ todo })
  //     return 'edit' in todo
  //   },
  // },
}
</script>
