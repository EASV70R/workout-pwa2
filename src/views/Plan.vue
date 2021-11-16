<template>
  <div class="plan">
    <h1 class="workout-header">{{ workout.title[lang] }}</h1>
    <p class="info">{{ durText[lang] }}: {{ workout.duration }}min</p>
    <p class="info">{{ diffText[lang] }}: {{ workout.diff }}/5</p>
    <img class="small-img" :src="require(`@/assets/${workout.category}${workout.id}.jpg`)" alt />
    <iframe
      width="415"
      height="262"
      :src="`https://www.youtube.com/embed/${workout.videoId}`"
      frameborder="0"
      allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen
    ></iframe>

    <div class="comments-container">
      <div class="new-comment-container" v-if="canPost">
        <label for="rating">{{ ratingText[lang] }}</label>
        <input type="number" name id="rating" min="1" max="5" placeholder="5" />
        <textarea name id="new-comment" cols="100" rows="3" v-model="newComment"></textarea>
        <!-- <button @click="postComment">post</button> -->
        <Button @click="postComment" :text="btnText[lang]" class="comment-btn" filled></Button>
        <!-- <p class="alert">alert</p> -->
      </div>

      <h3 class="comments-heading">{{ commentsText[lang] }}</h3>
      <Comment v-for="comment in comments" :comment="comment" :key="comment.id"></Comment>
    </div>
  </div>
</template>

<script>
import workouts from "@/workouts.json";
import Comment from "@/components/Comment.vue";
import Button from "@/components/Button.vue";
export default {
  data() {
    return {
      workout: null,
      comments: [],
      canPost: false,
      newComment: "",
      commentsText: { EN: "Comments", DA: "Kommentarer" },
      durText: { EN: "Duration", DA: "Varighed" },
      diffText: { EN: "Difficulty", DA: "Sværhedsgrad" },
      title: { EN: "Plans", DA: "Planer" },
      btnText: { EN: "Post comment", DA: "Skriv kommentar" },
      ratingText: { EN: "Your rating", DA: "Din vurdering" }
    };
  },
  props: ["lang"],
  components: {
    Comment,
    Button
  },
  methods: {
    postComment() {
      if (this.canPost) {
        this.comments.push({ user: "New USer", body: this.newComment });
        this.newComment = "";
      }
    }
  },
  mounted() {
    // console.log(workouts);

    this.workout = workouts[this.$route.params.id - 1];
    this.comments = this.workout.comments;
    document.title = `Play Fitness - ${this.title[this.lang]}`;
    const workoutss = JSON.parse(localStorage.getItem("reservedAppos"));
    for (let wo of workoutss) {
      // console.log(wo);
      // console.log(this.workout);
      if (
        wo.workoutId === this.workout.id &&
        wo.appoId === this.workout.appoId
      ) {
        this.canPost = true;
        break;
      }
    }
  }
};
</script>

<style>
.plan {
  padding-top: 85px;
  background: var(--main-bg-color);
  min-height: 140vh;
  color: white;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.workout-header {
  font-size: 50px;
}

.small-img {
  width: 415px;
  height: 262px;
  margin-bottom: 10px;
}

.info {
  font-size: 25px;
  padding: 5px;
}

#new-comment {
  width: 75vw;
  font-family: inherit;
  font-size: 20px;
  padding: 10px;
  resize: none;
  margin-top: 10px;
}

.comments-container {
  display: flex;
  flex-direction: column;
}

.comment-btn .cta-btn {
  margin-top: 10px;
  /* font-size: 20px; */
  /* width: 120px; */
}

.comments-heading {
  font-size: 25px;
  margin-top: 10px;
}

.new-comment-container {
  display: flex;
  flex-direction: column;
}

.alert {
  align-self: center;
}

#rating {
  width: 6%;
  font-size: 20px;
}
</style>
