<template>
  <div class="twoot-item" @click="favouriteTwoot(twoot.id)">
    <div class="user-profile__twoot">
      <div class="twoot-item__user">@{{ username }}</div>
      <div class="twoot-item__content">
        {{ twoot.content }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TwootItem",
  props: {
    username: {
      type: String,
      required: true,
    },
    twoot: {
      type: Object,
      required: true,
    },
  },
  methods: {
    favouriteTwoot(id) {
      this.$emit("favourite", id);
    },
  },
};
</script>

<style lang="scss" scoped>
.twoot-item {
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
  box-sizing: border-box;
  cursor: pointer;
  transition: all 0.25s ease;
  max-width: 100%;
  margin-bottom: 10px;

  .twoot-item__user {
    font-weight: bold;
  }

  .twoot-item__content {
    margin-top: 7px;
  }
  &:last-child {
    margin-bottom: 0;
  }
  &:hover {
    transform: scale(1.02, 1.02);
  }
}
</style>
