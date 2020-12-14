<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile__admin-badge" v-else-if="!user.isAdmin">
        Not Admin
      </div>
      <div class="user-profile__folower-count">
        <strong>Followers:</strong>{{ followers }}
      </div>
      <form
        class="user-profile__create-twoot"
        @submit.prevent="createNewTwoot"
        :class="{ '--exceeded': newTwootCharacterCount > 180 }"
      >
        <label for="newTwoot"
          ><strong>New Twoot</strong>

          ({{ newTwootCharacterCount }}/180)</label
        >
        <textarea
          id="newTwoot"
          cols="30"
          rows="4"
          v-model="newTwootContent"
        ></textarea>
        <div class="user-profile__create-twoot-type">
          <label for="newTwootType"><strong>Type:</strong></label>

          <select name="" id="newTwootType" v-model="selectedTwootType">
            <option
              :value="option.value"
              v-for="(option, index) in twootTypes"
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
    </div>

    <!-- カスタムイベント「favouriteMorimori」 -->
    <div class="user-profilte__twoots-wrapper">
      <TwootItem
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :twoot="twoot"
        :username="user.username"
        @favouriteMorimori="toggleFavourite"
        @deleteTwoot="deleteTwoot"
      />
    </div>
  </div>
</template>

<script>
  import TwootItem from "./TwootItem";

  export default {
    name: "UserProfile",
    components: { TwootItem },
    data() {
      return {
        newTwootContent: "",
        selectedTwootType: "instant",
        twootTypes: [
          { value: "draft", name: "Draft" },
          { value: "instant", name: "Instant Twoot" },
        ],
        isLoading: false,
        followers: 0,
        user: {
          id: 1,
          username: "_MithcellRomney",
          firstName: "Mitchell",
          lastName: "Rooney",
          email: "mitchellromney@theearchthissquire.com",
          isAdmin: true,
          twoots: [
            { id: 1, content: "Twotter is Amazing!" },
            {
              id: 2,
              content: "Don't forget to subscriber to The Earth is Square!",
            },
          ],
        },
      };
    },
    watch: {
      followers(newFollowerCount, oldFollowerCount) {
        if (oldFollowerCount < newFollowerCount) {
          console.log(`${this.user.username} has gained a followers`);
        }
      },
    },
    computed: {
      fullName() {
        return `${this.user.firstName} ${this.user.lastName}`;
      },
      newTwootCharacterCount() {
        return this.newTwootContent.length;
      },
    },
    methods: {
      followUser() {
        this.followers++;
      },
      unfollowUser() {
        this.followers--;
      },
      toggleFavourite(id, temp, obj) {
        console.log({ obj });
        console.log(temp);
        console.log(`Favourited Tweeet #${id}`);
      },
      createNewTwoot() {
        console.log("createNewTwoot");
        if (this.newTwootContent && this.selectedTwootType !== "draft") {
          this.user.twoots.unshift({
            // id: this.user.twoots.length + 1,
            id: Date.now(),
            content: this.newTwootContent,
          });
          this.newTwootContent = "";
        }
      },
      deleteTwoot(id) {
        console.log("deleteTwoot", { id });
        this.user.twoots = this.user.twoots.reduce((acc, value) => {
          value.id !== id && acc.push(value);
          return acc;
        }, []);
      },
    },
    mounted() {
      this.followUser();
    },
  };
</script>

<style lang="scss" scoped>
  .user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    /* width: 100%; */
    padding: 50px 5%;

    .user-profile__user-panel {
      display: flex;
      flex-direction: column;
      margin-right: 50px;
      padding: 20px;
      background-color: white;
      border-radius: 5px;
      border: 1px solid #dfe3e8;

      h1 {
        margin: 0;
      }
      .user-profile__admin-badge {
        background: rebeccapurple;
        color: white;
        border-radius: 5px;
        margin-right: auto;
        padding: 0 10px;
        font-weight: bold;
      }
      .user-profile__create-twoot {
        padding-top: 20px;
        display: flex;
        flex-direction: column;
        &.--exceeded {
          color: red;
          border-color: red;
          button {
            background-color: red;
            border: none;
            color: white;
          }
        }
      }
    }
  }
</style>
