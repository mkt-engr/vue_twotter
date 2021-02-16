<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ state.user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="state.user.isAdmin">
        Admin
      </div>
      <div class="user-profile__admin-badge" v-else-if="!state.user.isAdmin">
        Not Admin
      </div>
      <div class="user-profile__folower-count">
        <strong>Followers:</strong>{{ state.followers }}
      </div>
    </div>

    <!-- カスタムイベント「favouriteMorimori」 -->
    <div class="user-profilte__twoots-wrapper">
      <TwootItem
        v-for="twoot in state.user.twoots"
        :key="twoot.id"
        :twoot="twoot"
        :username="state.user.username"
        @favouriteMorimori="toggleFavourite"
        @deleteTwoot="deleteTwoot"
      />
    </div>
    <CreateTwootPanel @add-twoot="addTwoot" />
  </div>
</template>

<script>
  import TwootItem from "./TwootItem";
  import CreateTwootPanel from "./CreateTwootPanel";
  import { reactive } from "vue";
  export default {
    name: "UserProfile",
    components: { TwootItem, CreateTwootPanel },
    setup() {
      const state = reactive({
        followers: 0,
        user: {
          id: 1,
          username: "_MitchellRomney",
          firstName: "Mitchell",
          lastName: "Romney",
          email: "mitchellromney@theearthissquare.com",
          isAdmin: true,
          twoots: [
            { id: 1, content: "Twotter is Amazing" },
            {
              id: 2,
              content: "Don't forget to subscriber to The Earth is Square",
            },
          ],
        },
      });

      function addTwoot(newTwootContent) {
        console.log("addTwoot");
        state.user.twoots.unshift({
          id: state.user.twoots.length + 1,
          // id: Date.now(),
          content: newTwootContent,
        });
      }
      return {
        state,
        addTwoot,
      };
    },

    watch: {
      followers(newFollowerCount, oldFollowerCount) {
        if (oldFollowerCount < newFollowerCount) {
          console.log(`${this.user.username} has gained a followers`);
        }
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
    gap: 50px;
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
