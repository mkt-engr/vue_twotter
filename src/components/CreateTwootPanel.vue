<template>
  <form
    class="user-profile__create-twoot"
    @submit.prevent="createNewTwoot"
    :class="{ '--exceeded': newTwootCharacterCount > 180 }"
  >
    <label for="newTwoot"
      ><strong>New Twoot</strong> ({{ newTwootCharacterCount }}/180)</label
    >
    <textarea
      id="newTwoot"
      cols="30"
      rows="4"
      v-model="state.newTwootContent"
    ></textarea>
    <div class="user-profile__create-twoot-type">
      <label for="newTwootType"><strong>Type:</strong></label>

      <select name="" id="newTwootType" v-model="state.selectedTwootType">
        <option
          :value="option.value"
          v-for="(option, index) in state.twootTypes"
          :key="index"
        >
          {{ option.name }}
        </option>
      </select>
    </div>
    <button>
      Twoot!
    </button>
  </form>
</template>

<script>
  import { reactive, computed } from "vue";
  export default {
    name: "CreateTwootPanel",
    setup(props, ctx) {
      const state = reactive({
        newTwootContent: "",
        selectedTwootType: "instant",
        twootTypes: [
          { value: "draft", name: "Draft" },
          { value: "instant", name: "Instant Twoot" },
        ],
      });

      const newTwootCharacterCount = computed(() => {
        console.log(state.newTwootContent.length);
        return state.newTwootContent.length;
      });

      function createNewTwoot() {
        console.log("createNewTwoot");
        if (state.newTwootContent && state.selectedTwootType !== "draft") {
          ctx.emit("add-twoot", state.newTwootContent);
          state.newTwootContent = "";
        }
      }
      return {
        state,
        newTwootCharacterCount,
        createNewTwoot,
      };
    },

    data() {
      return {
        newTwootContent: "",
        selectedTwootType: "instant",
        twootTypes: [
          { value: "draft", name: "Draft" },
          { value: "instant", name: "Instant Twoot" },
        ],
      };
    },
    // computed: {
    //   newTwootCharacterCount() {
    //     return this.newTwootCharacterCount.length;
    //   },
    // },
    // methods: {
    //   createNewTwoot() {
    //     if (this.newTwootContent && this.selectedTwootType !== "draft") {
    //       this.$emit("add-twoot", this.newTwootContent);
    //       this.newTwootContent = "";
    //     }
    //   },
    // },
  };
</script>

<style lang="scss" scoped>
  .create-twoot-panel {
    margin-top: 20px;
    padding: 20px 0;
    display: flex;
    flex-direction: column;
    textarea {
      border: 1px solid #dfe3e8;
      border-radius: 5px;
    }
    .create-twoot-panel__submit {
      display: flex;
      justify-content: space-between;
      .create-twoot-type {
        padding: 10px 0;
      }
      button {
        padding: 5px 20px;
        margin: auto 0;
        border-radius: 5px;
        border: none;
        background-color: deeppink;
        color: white;
        font-weight: bold;
      }
    }
    &.--exceeded {
      color: red;
      border-color: red;
      .create-twoot-panel__submit {
        button {
          background-color: red;
          color: white;
        }
      }
    }
  }
</style>
