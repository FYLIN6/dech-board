<template>
  <nav class="menu-container">
    <div class="logo-div">
      <img
        src="../assets/1.png"
        alt="HeNan"
        class="logo"
        @click="this.$router.push('/')"
      />
    </div>
    <div class="navbar">
      <a class="navbar-a" @click="this.$router.push('/')"
        >Home
        <img class="home-icon" src="../assets/home-icon.png" alt="home-icon" />
      </a>
      <a class="navbar-a" @click="this.$router.push('/registeruser')"
        >Register
        <img
          class="register-icon"
          src="../assets/register-icon.png"
          alt="register-icon"
        />
      </a>
      <a class="navbar-a" @click="this.$router.push('/login')"
        >Login
        <img
          class="login-icon"
          src="../assets/login-icon.png"
          alt="login-icon"
        />
      </a>
      <a class="navbar-a" @click="this.$router.push('/test')"
        >Test
        <img class="test-icon" src="../assets/test-icon.png" alt="test-icon" />
      </a>
      <a class="navbar-a" @click="this.$router.push('/contacts')"
        >Contacts
        <img
          class="userlist-icon"
          src="../assets/users-alt.png"
          alt="userlist-icon"
        />
      </a>
      <a class="navbar-a" @click="this.$router.push('/conversationview')"
        >Conversation
        <img
          class="conversation-icon"
          src="../assets/conversation-icon.png"
          alt="c-icon"
        />
      </a>
      <a
        class="navbar-a"
        @click="
          deleteCookies();
          this.$router.push('/login');
        "
        >Log out
        <img
          class="conversation-icon"
          src="../assets/logout-icon.png"
          alt="c-icon"
        />
      </a>
    </div>
  </nav>
</template>
<script>
import { useCookies } from "vue3-cookies";
export default {
  name: "NavBar",
  setup() {
    const { cookies } = useCookies();
    return {
      cookies,
    };
  },
  methods: {
    deleteCookies() {
      this.cookies.remove("token");
      this.cookies.remove("messageboardid");
      this.cookies.remove("conversationID");
      this.cookies.remove("currentuser");
      this.cookies.remove("myCoookie");
    },
  },
};
</script>

<style scoped>
a {
  color: #506698;
  text-decoration: none;
}

* {
  font-family: "Basier circle", -apple-system, system-ui, "Segoe UI", Roboto,
    "Helvetica Neue", Arial, "Noto Sans", sans-serif, "Apple Color Emoji",
    "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji";
  font-weight: 600;
}

/* Top Navbar for testing */
.margin_class {
  background-color: black;
}

/* Left Side Bar CSS */
.navbar-a {
  --c: #00000000;
  /* the color */
  --b: 0.5em;
  /* border length*/
  --d: 1em;
  /* the cube depth */

  --_s: calc(var(--d) + var(--b));

  color: #000000bb;
  border: solid rgba(248, 248, 248, 0);
  border-width: var(--b) var(--b) var(--_s) var(--_s);
  background: conic-gradient(
        at left var(--d) bottom var(--d),
        #0000 90deg,
        rgba(255, 255, 255, 0.205) 0 225deg,
        rgba(255, 255, 255, 0.404) 0
      )
      border-box,
    conic-gradient(at left var(--_s) bottom var(--_s), #0000 90deg, var(--c) 0)
      0 100% / calc(100% - var(--b)) calc(100% - var(--b)) border-box;
  transform: translate(calc(var(--d) / -1), var(--d));
  clip-path: polygon(
    var(--d) 0%,
    var(--d) 0%,
    100% 0%,
    100% calc(100% - var(--d)),
    100% calc(100% - var(--d)),
    var(--d) calc(100% - var(--d))
  );
  transition: 0.2s;

  /* Side Bar Button position */
  position: relative;
  top: 20%;
  left: 0%;
  width: 10em;
  height: 60%;

  margin: 0em;
  padding: 0em;

  background-color: #92c4e0;
  /* Same color as sidebar background */
}

.navbar-a:hover {
  transform: translate(0, 0);
  clip-path: polygon(
    0% var(--d),
    var(--d) 0%,
    100% 0%,
    100% calc(100% - var(--d)),
    calc(100% - var(--d)) 100%,
    0% 100%
  );

  color: #ffffff;
  cursor: pointer;
}
.navbar {
  display: flex;
  justify-content: space-around;
  flex-direction: column;
  align-items: flex-start;
  position: absolute;
  top: 6em;
  left: 0em;
  width: 10em;
}

.menu-container {
  position: fixed;

  width: 11em;
  height: 100;
  margin: 0em;
  border: 0em;
  padding: 0em;
  top: 0%;
  bottom: 0%;
  margin-left: -0.5em;
  background: #92c4e0;
  box-shadow: 2px 2px 20px rgb(172, 197, 221);
}
/* Logo Inside the Side bar */
.logo {
  position: absolute;

  margin-top: -0.8em;
  left: 0em;

  width: 10.5em;
  height: 3em;
  border-radius: 0.65em;
  border-top-right-radius: 0.65em;
  border-bottom-right-radius: 0.65em;
  box-shadow: 3px 3px 15px rgba(101, 128, 134, 0.384);

  /* mix-blend-mode: multiply; */
}

.logo-div {
  position: absolute;

  top: 1em;
  left: 0em;

  width: 10em;
  height: 3.33em;
}

.logo-div:hover {
  cursor: pointer;
}

/* Register top blue bar */
.top {
  background-color: black;
  visibility: hidden;
}

/* Icons at left side bar*/
.home-icon {
  width: 1.2em;
  mix-blend-mode: multiply;
  position: absolute;
  left: 5%;
}

.register-icon {
  width: 1.2em;
  mix-blend-mode: multiply;
  position: absolute;
  left: 5%;
}

.login-icon {
  width: 1.2em;
  mix-blend-mode: multiply;
  position: absolute;
  left: 5%;
}

.test-icon {
  width: 1.2em;
  mix-blend-mode: multiply;
  position: absolute;
  left: 5%;
}

.userlist-icon {
  width: 1.2em;
  mix-blend-mode: multiply;
  position: absolute;
  left: 5%;
}

.conversation-icon {
  width: 1.2em;
  mix-blend-mode: multiply;
  position: absolute;
  left: 5%;
}
</style>