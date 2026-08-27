<template>
  <div class="box" :class="status">
    {{ name }} - {{ age }}
    >
    <span :class="['status', status]"> - {{ status }}</span>
  </div>
</template>

<script>
export default {
  name: "MyBox",
  props: {
    name: {
      type: String,
      default: "N/A",
    },
    age: {
      type: Number,
      required: true,
    },
    status: {
      type: String,
      validator: (value) => ["active", "inactive"].includes(value),
      default: "active",
    },
  },
};
</script>

<style>
.active {
  background-color: green;
}

.inactive {
  background-color: rgb(254, 5, 5);
}

.box {
  min-width: 30px;
  min-height: 100px;
  background-color: lightgray;
  align-items: center;
  justify-content: center;
  display: flex;
  margin: 10px;
  padding: 10px;
  border-radius: 12px;
}
</style>
