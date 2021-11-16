<template>
  <div class="card-container" id="card-container">
    <div
      @click="handleDivClick"
      class="card-img-container"
      id="card-img-container"
    >
      <img
        :src="require(`@/assets/${workout.category}${workout.id}.jpg`)"
        alt
      />
      <!-- <div class="img-bg" style="backgroundImage: `url(${require('@/assets/yoga1.png')})`"></div> -->
      <div class="img-filter">
        <span class="exercise-type">{{ workout.category }}</span>
      </div>
    </div>
    <div class="card-basic-info">
      <!-- <button class="more-info-btn">Vise</button> -->
      <span class="card-title">{{ workout.title[lang] }}</span>

      <div class="basic-info-container">
        <p>{{ cardInfoDiffText[lang] }}: {{ workout.diff }}/5</p>
        <p>{{ cardInfoRatingText[lang] }}: {{ workout.rating }}/5</p>
        <!-- <p>Duration: 1h45min</p> -->
      </div>
      <Button
        @click="handleClick"
        class="more-info-btn"
        :text="lang === 'EN' ? 'More' : 'Mere'"
        filled
      ></Button>
    </div>
    <div :class="['card-more-info', moreInfo ? 'show' : '']">
      <!-- <p>Info 1</p>
      <p>Info 2</p>
      <p>Info 3</p>-->
      <p>{{ durrText[lang] }}: {{ workout.duration }}min</p>
      <p>{{ appoText[lang] }}:</p>
      <div
        v-for="appointment in workout.appointments"
        :key="appointment.id"
        class="appointment-container"
      >
        <p>
          {{ appointment.date }} - {{ spotsText[lang] }}:
          {{ appointment.spotsLeft }}
          <span
            class="reserve-span"
            v-if="appointment.spotsLeft > 0 && checkReserved(appointment.id)"
            @click="reserveAppo(appointment.id)"
            >{{ reserveText[lang] }}</span
          >
          <!-- <span v-if="checkReserved(appointment.id)">Otkazi</span> -->
        </p>
      </div>
      <!-- <p>{{ appointment.date }}</p> -->
    </div>
  </div>
</template>

<script>
import Button from "./Button";
export default {
  data() {
    return {
      moreInfo: false,
      cardInfoDiffText: { EN: "Difficulty", DA: "Sværhedsgrad" },
      cardInfoRatingText: { EN: "Rating", DA: "Bedømmelse" },
      appoText: { EN: "Appointments", DA: "Tider" },
      durrText: { EN: "Duration", DA: "Varighed" },
      reserveText: { EN: "Reserve", DA: "Reservere" },
      spotsText: { EN: "Spots", DA: "Plads" },
    };
  },
  components: {
    Button,
  },
  props: ["workout", "lang"],
  methods: {
    handleClick() {
      console.log("Hi");
      this.moreInfo = !this.moreInfo;
    },
    handleDivClick() {
      this.$emit("onClick", this.workout);
    },
    reserveAppo(appoId) {
      let emptyArr = [];
      if (localStorage.getItem("reservedAppos") == null) {
        localStorage.setItem("reservedAppos", JSON.stringify(emptyArr));
      }
      let alreadyReserved = false;
      let reservedAppos = JSON.parse(localStorage.getItem("reservedAppos"));
      let reservation = {
        workoutId: this.workout.id,
        appoId,
      };
      reservedAppos.forEach((appo) => {
        // console.log(this.workout.id);
        // console.log("heya from foreach");
        if (appo.workoutId === this.workout.id && appo.appoId === appoId) {
          alreadyReserved = true;
        }
      });
      // console.log(alreadyReserved);
      if (!alreadyReserved) {
        reservedAppos.push(reservation);
        this.workout.appointments[appoId - 1].spotsLeft--;
      }
      localStorage.setItem("reservedAppos", JSON.stringify(reservedAppos));
    },
    checkReserved(appoId) {
      let reservedAppos = JSON.parse(localStorage.getItem("reservedAppos"));
      let alreadyReserved = false;
      if (reservedAppos) {
        reservedAppos.forEach((appo) => {
          // console.log(this.workout.id);
          // console.log("heya from foreach");
          if (appo.workoutId === this.workout.id && appo.appoId === appoId) {
            alreadyReserved = true;
          }
        });
      }

      return !alreadyReserved;
    },
  },
  computed: {},
};
</script>

<style>
.card-container {
  display: flex;
  flex-direction: column;
  position: relative;
  transition: all 0.09s ease-in;
  align-items: center;
  width: 415px;
  /* max-height: 330px; */
  max-height: 447px;
}

.card-container:hover {
  transform: scale(1.01);
}

.card-container:hover > .card-img-container {
  box-shadow: 10px 10px 16px -5px rgba(0, 0, 0, 0.5);
}

.card-container:hover > .card-basic-info {
  box-shadow: 0px 6px 16px -5px rgba(0, 0, 0, 0.5);
}

.card-basic-info {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: absolute;
  width: 400px;
  height: 90px;
  /* background-color: var(--second-bg-color); */
  /* background-color: #f1edff; */
  /* background-color: #777586; */
  background-color: #514f7c;
  /* bottom: 0; */
  left: 7px;
  border-radius: 4px;
  /* top: 65%; */
  top: 210px;
  /* border: 1px solid white; */
  transition: all 0.09s ease-in;
}

.basic-info-container {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

.basic-info-container p {
  padding: 10px;
  color: white;
}

.basic-info-container p:nth-child(2) {
  border-left: 1px solid white;
  /* border-right: 1px solid white; */
}

.card-img-container {
  /* border: 1px solid white; */
  transition: all 0.09s ease-in;
  position: relative;
  cursor: pointer;
}

.img-filter {
}

.card-more-info {
  background-color: white;
  position: relative;
  /* top: 120%; */
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: all 0.3s ease-in;
  /* z-index: 1; */
  max-height: 0;
  opacity: 0;
  width: 96%;
  border-bottom-left-radius: 4px;
  border-bottom-right-radius: 4px;
}

.card-more-info p {
  padding: 3px;
}

.card-more-info.show {
  /* display: flex; */
  max-height: 100vh;
  opacity: 1;
  z-index: 50;
}
.img-bg {
  /* background-image: url("/src/assets/yoga1.png"); */
}

.exercise-type {
  text-transform: uppercase;
}

.img-filter {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: white;
  font-size: 40px;
  border: 4px solid white;
  padding: 10px 30px;
  backdrop-filter: blur(15px);
}

.card-title {
  color: white;
  text-transform: uppercase;
  font-size: 20px;
  padding: 5px;
}

.more-info-btn .cta-btn {
  margin-top: 1px;
  width: 120px;
  height: 20px;
  /* padding: 0; */
  font-size: 16px;
  align-items: center;
  display: flex;
  justify-content: center;
}

.card-img-container {
  width: 415px;
  height: 262px;
}

.card-img-container img {
  width: 415px;
  height: 262px;
}

.reserve-span {
  cursor: pointer;
  margin-left: 10px;
  color: blue;
  text-decoration: underline;
}
</style>

