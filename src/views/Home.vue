<template>
  <div class="home">
    <div class="img-container">
      <img class="main-img" src="@/assets/cover-pic.png" alt />
      <img class="second-img" src="@/assets/pic2.png" alt />
    </div>
    <div class="text-play text">WORK</div>
    <div class="text-fcentar text">WorkOut App</div>
    <div class="text-adcopy text">
      {{ adcopy[lang] }}
      <div class="btn-container">
        <!-- <button class="cta-btn">{{ ctaBtn[lang] }}</button> -->
        <Button @click="handleSecondaryClick" class="main-btn" :text="secondaryBtn[lang]" />
        <Button @click="handlePrimaryClick" :text="primaryBtn[lang]" filled />
      </div>
    </div>
    <div class="white-box"></div>
    <div class="secondary-content">
      <!-- <h1>Our best</h1> -->
      <div class="cards-container">
        <!-- <WorkoutCard :workout="workouts[0]" :lang="lang" />
        <WorkoutCard :workout="workouts[4]" :lang="lang" />
        <WorkoutCard :workout="workouts[8]" :lang="lang" />
        <WorkoutCard :workout="workouts[7]" :lang="lang" />-->
        <WorkoutCard
          @onClick="handleClick(sortedWorkouts[index])"
          v-for="index in 3"
          :key="index"
          :workout="sortedWorkouts[index]"
          :lang="lang"
        ></WorkoutCard>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Button from "@/components/Button";
import WorkoutCard from "@/components/WorkoutCard";
import workouts from "@/workouts.json";

export default {
  name: "Home",
  components: {
    Button,
    WorkoutCard
  },
  data() {
    return {
      adcopy: {
        EN: "Find out why we are the best",
        DA: "Find ud af, hvorfor vi er de bedste"
      },
      secondaryBtn: { EN: "Find out more", DA: "Find ud af mere" },
      primaryBtn: { EN: "Choose a workout", DA: "Vælg en træningsplan" },
      workouts: [
        {
          id: 1,
          category: "yoga",
          title: "My Yoga",
          appointments: [
            { id: 1, text: "Info 1" },
            { id: 2, text: "Info 2" },
            { id: 3, text: "Info 3" }
          ]
        }
      ],
      sortedWorkouts: [],
      title: { EN: "Home", DA: "HJEM" }
    };
  },
  methods: {
    test() {
      console.log("hi");
    },
    handlePrimaryClick() {
      this.$router.push("/services");
    },
    handleSecondaryClick() {
      this.$router.push("/about");
    },
    handleClick(workout) {
      console.log(workout.id);
      this.$router.push(`plan/${workout.id}`);
    }
  },
  created() {
    this.workouts = [...workouts];
    console.log(this.workouts);
    this.sortedWorkouts = this.workouts.sort((a, b) => {
      return b.rating - a.rating;
    });
    document.title = `Workout - ${this.title[this.lang]}`;
  },
  props: ["lang"]
};
</script>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  z-index: 2;
  user-select: none;
  -webkit-user-drag: none;
}
.home {
  width: 100vw;
  /* height: 100vh; */
  background-color: var(--main-bg-color);
  font-family: "Rubik", sans-serif;
  /* overflow-x: hidden; */
  max-width: 100%;
}

.img-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
  max-width: 100%;
}

.img-container .main-img {
  /* width: 556px;
  height: 835px; */
  width: 511.52px;
  height: 768.2px;
  z-index: 2;
}

.main-img {
  transition: transform 0.09s ease-in;
}

.main-img:hover {
  /* transform: scale(1.1); */
}

.second-img {
  position: absolute;
  z-index: 1;
  left: 50%;
  bottom: 30%;
  /* overflow-x: hidden; */
}

.text-play {
  position: absolute;
  left: 960px;
  top: 475px;
  font-size: 300px;
  letter-spacing: 0.06em;
  color: white;
  /* font-family: "Ubuntu", sans-serif; */
}

.text-fcentar {
  position: absolute;
  left: 93px;
  top: 216px;
  color: white;
  font-size: 100px;
}

.text-fcentar::after {
  content: "";
  position: absolute;
  bottom: 0px;
  left: 45%;
  width: 442px;
  height: 8px;
  background-color: white;
  z-index: 3;
  border-bottom: solid 1px white;
}

.text-adcopy {
  position: absolute;
  left: 249px;
  top: 403px;
  font-size: 25px;
  line-height: 41px;
  text-align: right;
  color: white;
  width: 393px;
  height: 91px;
}

.btn-container {
  display: flex;
  width: 200%;
  justify-content: flex-start;
}

.main-btn {
  margin-right: 30px;
}

.white-box {
  position: absolute;
  height: 200px;
  width: 600px;
  background-color: var(--second-bg-color);
  top: 60%;
  z-index: 1;
}

.secondary-content {
  height: 100vh;
  width: 100vw;
  margin: 0;
  display: flex;
  /* flex-direction: ; */
  justify-content: center;
  background-color: var(--main-bg-color);
  max-width: 100%;
}

.cards-container {
  width: 100vw;
  /* padding: 20px; */
  /* display: flex;
    justify-content: space-evenly; */
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  justify-items: center;
  /* grid-gap: 10px; */
}
.text {
  color: var(--main-text-color);
}
</style>
