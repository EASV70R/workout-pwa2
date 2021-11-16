<template>
  <div class="about">
    <!-- <h1>This is an about page</h1> -->
    <h1>{{ teamText[lang] }}</h1>
    <p>Adorable Ape Studios </p>
    <p>Apdo & Torben</p>
    <h1>{{ awardsText[lang] }}</h1>
    <p>#1 Fitness</p>
    <h1>{{ phoneText[lang] }}</h1>
    <p>123456789</p>
    <h1>{{mapText[lang]}}</h1>
    <div class="about-container">
      <iframe
        src="https://www.google.com/maps/embed?pb=!1m14!1m8!1m3!1d36161.515418504074!2d8.4410368!3d55.4958748!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x464b20de0dc1b019%3A0xf131cc5b28fe1f27!2sErhvervsakademi%20SydVest!5e0!3m2!1sen!2sdk!4v1637025584393!5m2!1sen!2sdk"
        width="800"
        height="550"
        frameborder="0"
        style="border:0;"
        allowfullscreen
        aria-hidden="false"
        tabindex="0"
      ></iframe>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      teamText: { EN: "Our team", DA: "Vores team" },
      mapText: { EN: "Where are we?", DA: "Hvor er vi?" },
      phoneText: { EN: "Our phone", DA: "Vores telefon" },
      awardsText: { EN: "About us", DA: "Om Os" }
    };
  },
  props: ["lang"]
};
</script>

<style scoped>
.about {
  /* margin-top: 50px; */
  padding-top: 85px;
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  align-items: center;
  justify-content: center;
  background: var(--main-bg-color);
  color: white;
}

.about h1 {
  font-size: 40px;
  margin-top: 10px;
}

.about p {
  font-size: 20px;
}
</style>
