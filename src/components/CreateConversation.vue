
<template>
  <transition name="fade" appear>

    <div class="popup">
        <div class="popup-inner">
    <button
        type="button"
        class="btn-close popup-close"
        @click="TogglePopup()"
      >
        <span class="icon-cross"></span>
        <span class="visually-hidden"></span>
      </button>
     <br />
     <div class="ChooseUser">
     <v-container fluid>
        <table>
          <tr>
            <th>               
            </th>
          </tr>
          <div v-for="user in users" :key="user.username" >
            <tr>
              <td>
                <label for="">
                  <input
                    type="checkbox"
                    @click="
                      () => {
                        selectUsers(user.username);
                      }
                    "
                    :value="user.username"
                    id="checkbox-1-1"
                    class="regular-checkbox"
                  />
                </label>
              </td>
              <td>
                <label class="UserLabel">{{ user.username }}</label>
              </td>
            </tr>
          </div>
        </table>
      </v-container>
      </div>

    <div class="insert_message">

          
            <div class="message-create">
              <div class="message-textarea-div">
                <textarea
                  class="textarea-content"
                  name="content"
                  rows="3"
                  cols="55"
                  v-model="content"
                  placeholder="Your first Message..."
                >
                </textarea>
                <div class="send-button-div">
                  <button
                    class="button-81"
                    v-on:click="createMessage();
                TogglePopup();"
                    role="button"
                  >
                    Send Message
                  </button>
                </div>
              
            </div>
          
         

  </div>
    </div>
    </div>
  </div>
  </transition>
</template>

<script>
import axios from "axios";
import { useCookies } from "vue3-cookies";

export default {
   props: ["TogglePopup"],
  data() {
    
    return {
      content: "",
      users: [],
      recipients: [],
    };
  },
  computed: {},
  beforeMount() {
    this.getAllUsers();
  },
     setup() {
    const { cookies } = useCookies();
    return{
      cookies
    };},

  methods: {
    selectUsers(id) {
      //in here you can check what ever condition  before append to array.
      if (this.recipients.includes(id)) {
        const index = this.recipients.indexOf(id);

        this.recipients.splice(index, 1);
      } else {
        this.recipients.push(id);
      }
    },

    createDate() {
      for (let index = 0; index < this.messages.length; index++) {
        let newDate = new Date(this.messages[index].time);
        this.messages[index].time = newDate.toTimeString();
        console.log("CreateDate");
      }
    },
    async getAllUsers() {
      let headers = {
        "Content-Type": "application/json",
        authorization: this.cookies.get("token"),
      };
      let uri = "http://localhost:8090/getUsers";

      let response = await axios
        .get(uri, { headers: headers })
        .then((response) => {
          this.users = response.data;
        });
      console.log(response);
    },

    //Create a message
    async createMessage() {
      console.log(this.recipients);
      if (this.recipients[0]) {
        let result = await axios.post(
          "http://localhost:8090/message/create",
          {
            recipients: this.recipients,
            content: this.content,
          },
          {
            headers: {
              authorization: this.cookies.get("token"),
            },
          }
        );
        console.log(result);
      } else {
        console.log("Else");
      }
    },
  },
};
</script>

<style scoped lang="scss">
.ChooseUser{

width:20em;
height: 20em;
background-color: rgba(244, 247, 255, 255);

border-radius: 1em;
padding: 1em;
margin-bottom: 2em;
 overflow: scroll;
  overflow-x: hidden;
}

.ChooseUser::-webkit-scrollbar-track {
  background-color: #f5f5f5;
  border-radius: 1em;
}
.ChooseUser::-webkit-scrollbar {
  background-color: #f5f5f5;
  border-radius: 1em;
  width: 1.6em;
}
.ChooseUser::-webkit-scrollbar-thumb {
  background-color: #e1e7f7;
  border-radius: 1em;
}


.popup {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 99;
  background-color: rgba(0, 0, 0, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-inner {
  background: white;
  padding: 32px;
  border-radius: 10px;
}



@mixin cross($size: 20px, $color: currentColor, $thickness: 1px) {
  margin: 0;
  padding: 0;
  border: 0;
  background: none;
  position: relative;
  width: $size;
  height: $size;

  &:before,
  &:after {
    content: "";
    position: absolute;
    top: ($size - $thickness) / 2;
    left: 0;
    right: 0;
    height: $thickness;
    background: $color;
    border-radius: $thickness;
  }

  &:before {
    transform: rotate(45deg);
  }

  &:after {
    transform: rotate(-45deg);
  }

  span {
    display: block;
  }
}



.btn-close {
  margin: 0;
  border: 0;
  padding: 0;
  background: rgb(255, 150, 150);
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  transition: all 150ms;
  float: right;

  .icon-cross {
    @include cross(20px, #fff, 6px);
  }

  &:hover,
  &:focus {
    transform: rotateZ(90deg);
    background: rgb(253, 78, 78);
  }
}


.UserLabel {
  font-size: 1.1em;
  display: block;
}

.regular-checkbox {
  -webkit-appearance: none;
  background-color: #ffffff;
  border: 1px solid rgba(120, 194, 255, 0.966);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05),
    inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05);
  padding: 9px;
  border-radius: 3px;
  display: inline-block;
  position: relative;
  cursor: pointer;
}

.regular-checkbox:active,
.regular-checkbox:checked:active {
  box-shadow: 0 1px 2px rgba(120, 194, 255, 0.966),
    inset 0px 1px 3px rgba(120, 194, 255, 0.966);
}

.regular-checkbox:checked {
  background-color: #ffffff;
  border: 1px solid rgba(120, 194, 255, 0.966);
  box-shadow: 0 1px 2px rgba(120, 194, 255, 0.966),
    inset 0px -15px 10px -12px rgba(120, 194, 255, 0.966),
    inset 15px 10px -12px rgba(120, 194, 255, 0.966);
  color: #99a1a7;
}

.regular-checkbox:checked:after {
  content: "\2714";
  font-size: 14px;
  position: absolute;
  top: 0px;
  left: 3px;
  color: #4fb3ff;
}
.message-create {
  width: 40em;

  padding: 0.5em;
  margin: auto;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  border-radius: 10px;
  background-color: rgba(244, 247, 255, 255);
  box-shadow: rgba(50, 50, 93, 0.25) 0px 13px 27px -5px,
    rgba(0, 0, 0, 0.3) 0px 8px 16px -8px;
}
.message-textarea-div {
  height: 5em;
}
.textarea-content {
  width: 70%;
  height: 55%;
  margin-left: 1em;
  margin-top: 1.2em;
  resize: none;
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  border-radius: 5px;
  border: none;
  outline: none;
  float: left;
  padding-left: 3px;
  padding-right: 3px;
}
.send-button-div {
  width: 22%;
  height: 100%;
  float: right;
  padding-right: 1em;
  display: flex;
  justify-content: center;
  align-items: center;
}
/* CSS */
.button-81 {
  width: 100vh;
  background-color: rgba(244, 247, 255, 255);
  border: 0 solid #e2e8f0;
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