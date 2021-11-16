<template>
  <div class="profile">
    <h1>{{ profileText[lang] }}</h1>
    <div class="workouts-woo" v-if="myWorkouts.length > 0">
      <div v-for="(workout, index) in myWorkouts" :key="index" class="remove-workout-container">
        <WorkoutCard :workout="workout" :lang="lang" @onClick="handleClick(workout)"></WorkoutCard>
        <Button @click="removeWorkout(workout)" :text="btnText[lang]" class="remove-btn"></Button>
      </div>
    </div>
    <div class="no-workout" v-else>
      <p>{{ noWorkoutsText[lang] }}</p>
    </div>
  </div>
</template>

<script>
import WorkoutCard from "@/components/WorkoutCard.vue";
import Button from "@/components/Button.vue";
import workouts from "@/workouts.json";
export default {
  data() {
    return {
      workouts: [],
      myWorkouts: [],
      profileText: { EN: "Profile", DA: "Profil" },
      btnText: { EN: "Remove", DA: "Fjerne" },
      noWorkoutsText: {
        EN: "No training plan (choose one)",
        DA: "Intet træningsplan (vælg en)"
      }
    };
  },
  props: ["lang"],
  components: {
    WorkoutCard,
    Button
  },
  methods: {
    removeWorkout(workoutToRemove) {
      let storedWorkouts = JSON.parse(localStorage.getItem("reservedAppos"));
      storedWorkouts = storedWorkouts.filter(workout => {
        return (
          workout.workoutId !== workoutToRemove.id ||
          workout.appoId !== workoutToRemove.appoId
        );
      });
      localStorage.setItem("reservedAppos", JSON.stringify(storedWorkouts));
      this.myWorkouts = this.myWorkouts.filter(workout => {
        console.log(workoutToRemove);
        return (
          workout.id !== workoutToRemove.id ||
          workout.appoId !== workoutToRemove.appoId
        );
      });
      // this.myWorkouts = this.myWorkouts.filter(workout => {
      //   if (workout.id === workoutToRemove.id) {
      //     if (workout.appoId === workoutToRemove.appoId) {
      //       return false;
      //     } else {
      //       return true;
      //     }
      //   }
      // });
    },
    handleClick(workout) {
      this.$router.push(`plan/${workout.id}`);
    }
  },
  created() {
    this.workouts = [...workouts];
    let storedWorkouts = JSON.parse(localStorage.getItem("reservedAppos"));
    if (storedWorkouts !== null) {
      //   this.workouts.forEach(workout => {
      //     storedWorkouts.forEach(stored => {
      //       if (stored.workoutId === workout.id) {
      //         workout.appoId = stored.appoId;
      //         this.myWorkouts.push(workout);
      //       }
      //     });
      //   });
      storedWorkouts.forEach(stored => {
        this.workouts.forEach(workout => {
          if (stored.workoutId === workout.id) {
            workout.appoId = stored.appoId;
            this.myWorkouts.push(workout);
          }
        });
      });
    }
    console.log(this.myWorkouts);
    document.title = `Play Fitness - ${this.profileText[this.lang]}`;
  }
};
</script>

<style>
.profile {
  padding-top: 85px;
  background: var(--main-bg-color);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.profile h1 {
  color: white;
  font-size: 40px;
  margin-bottom: 10px;
}

.remove-workout-container {
  display: flex;
  justify-content: center;
  align-items: center;
}

.remove-btn .cta-btn {
  padding: 10px;
  /* font-size: 1px; */
  margin-left: 40px;
}

.no-workout p {
  font-size: 50px;
  color: white;
}
</style>
