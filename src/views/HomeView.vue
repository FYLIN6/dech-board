<template>
  <div class="sticky">
    <UserObjects />
  </div>
  <!-- <div class="sticky"> -->
  <LeftSideMenu />

  <!-- </div> -->
  <MessageBoardSelection @changeBoard="changeBoard" />
  <button
    class="button-81" id="fixedbuttonBoard"
    @click="() => TogglePopupSecond('buttonTrigger')"
    role="button"
  >
    Create Board
  </button>
  <button
    class="button-81" id="fixedbuttonPost"
    @click="() => TogglePopup('buttonTrigger')"
    role="button"
  >
    Create Post
  </button>

  <!-- <button
  <LeftSideMenu />
  <MessageBoardSelection @changeBoard="changeBoard" />

  <button
    class="icon-btn add-btn"
    id="message-board"
    @click="() => TogglePopupSecond('buttonTrigger')"
  >
    <div class="add-icon"></div>
    <div class="btn-txt">Create Board</div>
  </button> -->

  <MessageBoardCreator
    v-if="popupTriggersSecond.buttonTrigger"
    :TogglePopupSecond="() => TogglePopup2Second('buttonTrigger')"
  />

  <div class="home">
    <div v-show="false">
      <MessageBoard />
    </div>
    <div class="" style="text-align: center; margin-top: 5%">
      <!-- <button
        class="icon-btn add-btn"
        @click="() => TogglePopup('buttonTrigger')"
        id="home"
      >
        <div class="add-icon"></div>
        <div class="btn-txt">Add Post</div>
      </button> -->
    </div>

    <CreatePost
      @Messageboard="messageboard"
      v-if="popupTriggers.buttonTrigger"
      :TogglePopup="() => TogglePopup2('buttonTrigger')"
    >
    </CreatePost>
    <AlluserList></AlluserList>
  </div>
</template>

<script>
// @ is an alias to /src
import { ref } from "vue";
import MessageBoard from "@/components/MessageBoard.vue";
import CreatePost from "@/components/CreatePost.vue";
import LeftSideMenu from "@/components/LeftSideMenu.vue";
import MessageBoardCreator from "@/components/MessageBoardCreator.vue";
import MessageBoardSelection from "@/components/MessageBoardSelection.vue";
import axios from "axios";
import UserObjects from "@/components/UserObjects.vue";
import { useCookies } from "vue3-cookies";

export default {
  setup() {
    const { cookies } = useCookies();
    const popupTriggersSecond = ref({
      buttonTriggerSecond: false,
    });

    const TogglePopupSecond = (trigger) => {
      popupTriggersSecond.value[trigger] = !popupTriggersSecond.value[trigger];
    };

    const TogglePopup2Second = (trigger) => {
      popupTriggersSecond.value[trigger] = !popupTriggersSecond.value[trigger];
    };

    const popupTriggers = ref({
      buttonTrigger: false,
    });

    const TogglePopup = (trigger) => {
      popupTriggers.value[trigger] = !popupTriggers.value[trigger];
    };

    const TogglePopup2 = (trigger) => {
      popupTriggers.value[trigger] = !popupTriggers.value[trigger];
    };

    return {
      popupTriggers,
      TogglePopup,
      TogglePopup2,
      TogglePopupSecond,
      TogglePopup2Second,
      popupTriggersSecond,
      cookies,
    };
    //
  },

  beforeMount() {
    this.saveBoards();
  },

  data() {
    return {
      boardName: "Dech-Board",
      messageboards: [],
    };
  },

  components: {
    MessageBoard,
    CreatePost,
    LeftSideMenu,
    MessageBoardSelection,
    MessageBoardCreator,
    UserObjects,
  },
  methods: {
    saveBoards() {
      let headers = {
        "Content-Type": "application/json",
        authorization: this.cookies.get("token"),
      };

      let uri = "http://localhost:8090/messageboard/get";

      let response = axios.get(uri, { headers: headers }).then((response) => {
        this.messageboards = response.data;
      });
      console.log(response);
    },

    changeBoard(title) {
      console.log(`hello world ${title}`);
      this.boardName = title;
    },
    nextBoard() {
      console.log("Go to next Board!");

      let currentBoardId = this.cookies.get("messageboardid");
      console.log(this.messageboards.length);
      for (let index = 0; index < this.messageboards.length; index++) {
        console.log(
          this.messageboards[index].messageBoardId + "--=--" + currentBoardId
        );
        if (this.messageboards[index].messageBoardId == currentBoardId) {
          console.log("Old" + this.messageboards[index].messageBoardId);
          let newIndex = index;
          this.cookies.set(
            "messageboardid",
            this.messageboards[newIndex + 1].messageBoardId,
            0
          );
          return;
        } else if (currentBoardId == 0) {
          console.log("Neew" + this.messageboards[index].messageBoardId);
          this.cookies.set(
            "messageboardid",
            this.messageboards[index].messageBoardId,
            0
          );
          return;
        }
      }
    },
  },
};
</script>

<style scoped>
#home {
  position: absolute;
  top: 7em;
  left: -30em;
  width: 50px;
  height: 50px;
  border-radius: 25px;
}

#message-board {
  position: absolute;
  top: 7em;
  left: 12em;
  width: 50px;
  height: 50px;
  border-radius: 25px;
}
.home {
  position: absolute;
  top: 20%;
  left: 50%;
  width: auto;
  height: auto;
}

.sticky {
  position: -webkit-sticky;
  position: sticky;
  top: 0;
}
.icon-btn add-btn {
  position: absolute;

  width: 100em;
  height: 10em;
}

.main-hr {
  width: 30%;
  border: none;
  border-top: 1px solid #c3c3c3;
}
.icon-btn {
  width: 50px;
  height: 50px;
  border: 1px solid #cdcdcd;
  background: white;
  border-radius: 25px;
  overflow: hidden;
  position: relative;
  transition: width 0.2s ease-in-out;
}
.add-btn:hover {
  width: 120px;
  cursor: pointer;
}
.add-btn::before,
.add-btn::after {
  transition: width 0.2s ease-in-out, border-radius 0.2s ease-in-out;
  content: "";
  position: absolute;
  height: 4px;
  width: 10px;
  top: calc(50% - 2px);
  background: rgb(117, 232, 255);
}
.add-btn::after {
  right: 14px;
  overflow: hidden;
  border-top-right-radius: 2px;
  border-bottom-right-radius: 2px;
}
.add-btn::before {
  left: 14px;
  border-top-left-radius: 2px;
  border-bottom-left-radius: 2px;
}
.icon-btn:focus {
  outline: none;
}
.btn-txt {
  opacity: 0;
  transition: opacity 0.2s;
}
.add-btn:hover::before,
.add-btn:hover::after {
  width: 4px;
  border-radius: 2px;
}
.add-btn:hover .btn-txt {
  opacity: 1;
}
.add-icon::after,
.add-icon::before {
  transition: all 0.2s ease-in-out;
  content: "";
  position: absolute;
  height: 20px;
  width: 2px;
  top: calc(50% - 10px);
  background: rgb(117, 232, 255);
  overflow: hidden;
}
.add-icon::before {
  left: 22px;
  border-top-left-radius: 2px;
  border-bottom-left-radius: 2px;
}
.add-icon::after {
  right: 22px;
  border-top-right-radius: 2px;
  border-bottom-right-radius: 2px;
}
.add-btn:hover .add-icon::before {
  left: 15px;
  height: 4px;
  top: calc(50% - 2px);
}
.add-btn:hover .add-icon::after {
  right: 15px;
  height: 4px;
  top: calc(50% - 2px);
}

.arrow {
  width: 1em;
  margin-left: 5em;
  margin-right: 5em;
}
#fixedbuttonBoard {
    position: fixed;
    bottom: 10em;
    right: 10em; 
}
#fixedbuttonPost {
    position: fixed;
    bottom: 17em;
    right: 10em; 
}

.button-81 {
  /* position: sticky; */
  
  width: 10em;
  height: 5em;
  margin-top: 5em;
  margin-left: 65em;
  background-color: rgba(244, 247, 255, 255);
  border: 1 solid #5f88bd;
  border-radius: 1.5rem;
  box-sizing: border-box;
  color: #0d222a;
  cursor: pointer;
  display: inline-block;
  font-family: "Basier circle", -apple-system, system-ui, "Segoe UI", Roboto,
    "Helvetica Neue", Arial, "Noto Sans", sans-serif, "Apple Color Emoji",
    "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
  font-size: 1.1rem;
  font-weight: 600;
  line-height: 1;
  padding: 0.8rem 0.5rem;
  text-align: center;
  text-decoration: none rgba(244, 247, 255, 255) solid;
  text-decoration-thickness: auto;
  transition: all 0.1s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0px 1px 2px rgba(118, 162, 255, 0.25);
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button-81:hover {
  background-color: rgba(102, 194, 247, 0.25);
  color: rgb(0, 0, 0);
}

@media (min-width: 768px) {
  .button-81 {
    font-size: 0.9rem;
    padding: 0.8rem 0.5rem;
  }
}

@media (min-width: 768px) {
  .button-81 {
    font-size: 0.9rem;
    padding: 0.8rem 0.5rem;
  }
}
</style>