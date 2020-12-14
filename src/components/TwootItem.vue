<template>
  <div class="twoot-item" @click="favouriteTwoot(twoot.id)">
    <div class="user-profile__twoot">
      <div class="twoot-item__user">@{{ username }}</div>
    </div>
    <div class="twoot-item__Content">
      {{ twoot.content }}
      id:{{ twoot.id }}
    </div>
    <button @click.stop="deleteTwootMethod(twoot.id)">Delete</button>
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
        this.$emit("favouriteMorimori", id, "aaa", { a: 1 }); //親で受け取る名前,親に渡したい変数
      },
      deleteTwootMethod(id) {
        this.$emit("deleteTwoot", id);
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
    &:hover {
      transform: scale(1.1);
    }
    .twoot-item__user {
      font-weight: bold;
    }
  }
</style>
