<template>
  <div class="message" :class="{ fromUser: owner }">
    {{ message.text }}
    <span class="sender">from UID {{ message.sender }}</span>
  </div>
</template>

<script>
export default {
  props: ["message", "owner"],
};
</script>

<style scoped>
.message {
  background-color: #dddddd;
  color: black;
  margin: 2px 10px 2px auto;
  border-radius: 5px;
  padding: 12px;
  display: inline-block;
}
.fromUser {
  margin: 2px auto 2px 10px;
  background-color: #91bbff;
}
.sender {
  font-size: 0.5rem;
}
</style>