<template>
  <div class="appointments">
    <div class="appo-form">
      <h1>{{ type[$route.params.type][lang] }}</h1>
      <label for="name">{{ nameText[lang] }}</label>
      <input type="text" name id="name" v-model="name" />
      <label for="surname">{{ surnameText[lang] }}</label>
      <input type="text" name id="surname" v-model="surname" />
      <label for="email">Email</label>
      <input type="email" name id="email" />
      <label for="date">{{ dateText[lang] }}</label>
      <input type="date" name id="date" />
      <label for="problem">{{ problemText[lang] }}</label>
      <textarea name id="problem" cols="30" rows="10"></textarea>
    </div>
    <button @click="savePdf" class="appo-btn">Submit</button>
  </div>
</template>

<script>
import jsPDF from "jspdf";
import html2canvas from "html2canvas";
export default {
  data() {
    return {
      type: {
        contact: { EN: "Contact Us", DA: "Kontakt Os" }
      },
      nameText: { EN: "Name", DA: "Navn" },
      surnameText: { EN: "Surname", DA: "Efternavn" },
      dateText: { EN: "Date", DA: "Dato" },
      problemText: { EN: "Problem Description", DA: "Problem Beskrivelse" },

      title: { EN: "Appointments", DA: "Tider" }
    };
  },
  props: ["lang"],
  methods: {
    savePdf() {
      const filename = `${this.name} ${this.surname}`;

      html2canvas(document.querySelector(".appo-form"), { scale: 2 }).then(
        canvas => {
          let pdf = new jsPDF("p", "mm", "a4");
          pdf.addImage(canvas.toDataURL("image/png"), "PNG", 50, 20); //, 0, 0, 211, 298
          pdf.save(filename);
        }
      );
    }
  },
  created() {
    document.title = `Play Fitness - ${this.title[this.lang]} - ${
      this.type[this.$route.params.type][this.lang]
    }`;
  }
};
</script>

<style scoped>
.appointments {
  padding-top: 85px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.appo-form {
  display: flex;
  flex-direction: column;
}

.appointments h1 {
  margin-bottom: 40px;
  margin-top: 10px;
}

.appointments input {
  padding: 10px;
  margin-bottom: 10px;
}

.appointments label {
  font-size: 20px;
  margin-bottom: 5px;
}

.appo-btn {
  padding: 10px 20px;
  font-size: 30px;
  margin-top: 20px;
}
</style>
