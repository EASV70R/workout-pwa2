<template>
  <div :class="['nav', scrollPosition > 50 ? 'scrolled' : '']">
    <!-- <button @click="changeTheme">asd</button> -->
    <!-- <button @click="savePdf">PDF</button> -->

    <router-link class="nav-link" to="/">{{ linkHome[lang] }}</router-link>
    <router-link class="nav-link" to="/services">{{ link1[lang] }}</router-link>
    <router-link class="nav-link" to="/appointments/contact">{{ link2[lang] }}</router-link>


    <router-link class="nav-link" to="/profile">{{ link3[lang] }}</router-link>
    <router-link class="nav-link" to="/about">{{ link4[lang] }}</router-link>
    <div class="lang-toggle-container">
      <div :class="['nav-link', isEnLang ? 'grayed-out' : '']">DA</div>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="27"
        height="27"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
        class="feather feather-toggle-left lang-toggle-icon"
        @click="changeLang"
      >
        <rect x="1" y="5" width="22" height="14" rx="7" ry="7" />
        <circle class="circle" :cx="cx" cy="12" r="3" />
      </svg>
      <div :class="['nav-link', isEnLang ? '' : 'grayed-out']">EN</div>
      <!-- <button class="lang-toggle nav-link" @click="changeLang">{{ lang }}</button>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
        class="feather feather-globe lang-toggle-icon"
      >
        <circle cx="12" cy="12" r="10" />
        <line x1="2" y1="12" x2="22" y2="12" />
        <path
          d="M12 2a15.3 15.3 0 0 1 4 10 15.3 15.3 0 0 1-4 10 15.3 15.3 0 0 1-4-10 15.3 15.3 0 0 1 4-10z"
        />
      </svg>-->
    </div>
  </div>
</template>

<script>
//Libs
import jsPDF from "jspdf";
import html2canvas from "html2canvas";

export default {
  data() {
    return {
      lang: "DA",
      linkHome: { EN: "HOME", DA: "HJEM" },
      link1: { EN: "SERVICES", DA: "SERVICE" },
      link2: { EN: "BOOK A SESSION", DA: "BESTIL EN SESSION" },
      link3: { EN: "MY PROFILE", DA: "MIN PROFIL" },
      link4: { EN: "ABOUT US", DA: "OM OS" },
      ddLink1: { EN: "WORKOUTS", DA: "TRÆNING" },
      ddLink3: { EN: "CONTACT US", DA: "KONTAKT OS" },
      cx: 8,
      scrollPosition: null
    };
  },
  methods: {
    changeLang() {
      console.log(this.lang);
      if (this.lang === "DA") {
        this.lang = "EN";
        this.cx = 16;
      } else {
        this.lang = "DA";
        this.cx = 8;
      }
      this.$emit("langChange", this.lang);
    },
    updateScroll() {
      this.scrollPosition = window.scrollY;
    },
    changeTheme() {
      let bodyStyles = document.body.style;
      if (bodyStyles.getPropertyValue("--main-bg-color") !== "#f5fafa") {
        bodyStyles.setProperty("--main-bg-color", "#f5fafa");
        bodyStyles.setProperty("--second-bg-color", "#acd1e9");
        bodyStyles.setProperty("--main-text-color", "black");
      } else {
        bodyStyles.setProperty("--main-bg-color", "#21234b");
        bodyStyles.setProperty("--second-bg-color", "#10153b");

        bodyStyles.setProperty("--main-text-color", "white");
      }
    },
    savePdf() {
      const filename = "PdfTest.pdf";

      html2canvas(document.querySelector("body")).then(canvas => {
        let pdf = new jsPDF("p", "mm", "a4");
        pdf.addImage(canvas.toDataURL("image/png"), "PNG", 0, 0, 211, 298);
        pdf.save(filename);
      });
    }
  },
  mounted() {
    window.addEventListener("scroll", this.updateScroll);
  },
  computed: {
    isEnLang() {
      return this.lang === "EN";
    }
  }
};
</script>

<style>
.nav {
  position: fixed;
  display: flex;
  background-color: var(--main-bg-color);
  /* background-color: var(--main-bg-color); */
  height: 85px;
  width: 100vw;
  justify-content: flex-end;
  padding: 30px;
  z-index: 3;
  /* top: 0; */
  transition: background-color 0.09s ease-in;
}

.scrolled {
  background-color: var(--second-bg-color);
}

.nav-link {
  color: white;
  text-decoration: none;
  font-family: "Rubik", sans-serif;
  letter-spacing: 0.14em;
  margin-right: 20px;
  margin-left: 20px;
  font-size: 20px;
  position: relative;
}

.grayed-out {
  opacity: 0.5;
}

.nav-link::before {
  content: "";
  position: absolute;
  background: white;
  top: 100%;
  left: 0;
  right: 0;
  bottom: 0;
  height: 2px;
  width: 0;
  z-index: -1;
  transition: 0.2s ease-in;
}

.nav-link:hover::before {
  right: 100%;
  width: 100%;
  z-index: 1;
}

.dropdown-container {
  position: relative;
}

.lang-toggle-container {
  display: flex;
}

.lang-toggle {
  background: none;
  background-color: rgba(0, 0, 0, 0);
  cursor: pointer;
  border: none;
}

.lang-toggle:focus {
  outline: none;
}

.lang-toggle-icon {
  color: white;
  size: 30px;
  transition: 0.09s ease-in;
  cursor: pointer;
}

.dropdown-menu {
  display: none;
  opacity: 0;
  position: absolute;
  flex-direction: column;
  background-color: var(--second-bg-color);
  border-radius: 4px;
  box-shadow: 7px 10px 24px -9px rgba(255, 255, 255, 0.5);
  /* border-left: 1px solid black;
  border-right: 1px solid black;
  border-bottom: 1px solid black; */
  z-index: 5;
  /* height: 20px; */
  top: 25px;
  transition: all 0.9s ease-in;
}

.dropdown-item {
  position: relative;
  /* margin-top: 10px; */
  padding: 10px;
  z-index: 3;
  /* text-align: center; */
  /* margin-right: 0; */
}

.dropdown-item:first-of-type {
  margin-top: 10px;
}

.dropdown-item:last-of-type {
  margin-bottom: 10px;
}

.dropdown-container:hover .dropdown-menu {
  display: flex;
  opacity: 1;
}

.circle {
  transition: all 0.09s ease-in;
}

.router-link-exact-active {
  font-weight: bold;
  color: #7672b3;
}

@media (max-width: 1040px) {
  .nav {
    position: relative;
    flex-direction: column;
    height: 50vh;
    justify-content: center;
    align-items: center;
  }
}
</style>
