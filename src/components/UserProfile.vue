<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
      <div class="user-profile__follower-count">
        <strong> Followers: </strong>{{ followers }}
      </div>
      <div>
        <form
          class="user-profile__create-twoot"
          @submit.prevent="createNewTwoot"
          :class="{ exceeded: newTwootCharacterCount > 180 }"
        >
          <label for="newTwoot"
            ><strong>New Twoot</strong> ({{
              newTwootCharacterCount
            }}/180)</label
          >
          <textarea
            id="newTwoot"
            rows="4"
            v-model="newTwootContent"
            style="resize: none; font-size: 16px"
          ></textarea>

          <div class="user-profile__create-twoot-type">
            <label for="newTwootType"><strong>Type:</strong></label>
            <select id="newTwootType" v-model="selectedTwootType">
              <option
                :value="option.value"
                v-for="(option, index) in twootTypes"
                :key="index"
              >
                {{ option.name }}
              </option>
            </select>
          </div>
          <button class="twootBtn">Twoot!</button>
        </form>
      </div>
    </div>
    <div class="user-profile__twoots-wrapper">
      <TwootItem
        v-for="twoot in user.twoots"
        :key="twoot.id"
        :username="user.username"
        :twoot="twoot"
        @favourite="toggleFavourite"
      />
    </div>
  </div>
</template>

<script>
import TwootItem from "./TwootItem";
export default {
  name: "App",
  components: { TwootItem },
  data() {
    return {
      newTwootContent: "",
      selectedTwootType: "instant",
      twootTypes: [
        {
          value: "draft",
          name: "Draft",
        },
        {
          value: "instant",
          name: "Instant Twoot",
        },
      ],
      followers: 0,
      user: {
        id: 1,
        username: "_Gandalf",
        firstName: "Gandalf",
        lastName: "The Grey",
        email: "umusayt@gmail.com",
        isAdmin: true,
        twoots: [
          { id: 1, content: "Twooter is amazing!" },
          { id: 2, content: "Gandalf is awesome!" },
        ],
      },
    };
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    },
  },
  computed: {
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    },
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavourite(id) {
      console.log(`Favourited twoot #${id}`);
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType != "draft") {
        this.user.twoots.unshift({
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        });
        this.newTwootContent = "";
      }
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
      background: deepskyblue;
      margin-top: 7px;
      color: white;
      border-radius: 5px;
      margin-right: auto;
      padding: 3px 10px;
      font-weight: 600;
    }
    .user-profile__follower-count {
      margin-top: 7px;
    }
    .twootBtn {
      margin-top: 20px;
      background-color: deepskyblue;
      border: none;
      cursor: pointer;
      font-size: 17px;
      color: white;
      border-radius: 5px;
    }

    .twootBtn:hover {
      background-color: rgb(0, 191, 255);
      transition: 0.2s;
      transform: scale(1.05, 1.05);
    }
  }
  .user-profile__create-twoot {
    padding-top: 20px;
    display: flex;
    flex-direction: column;
    &.exceeded {
      color: red;
    }
  }

  .user-profile__create-twoot-type {
    padding-top: 20px;
  }
  select {
    background-color: deepskyblue;
    color: white;
    border-radius: 5px;
    margin-left: 5px;
    border: none;
    font-size: 15px;
    padding: 2px;

    &:focus {
      outline: none;
    }
  }
}
</style>
