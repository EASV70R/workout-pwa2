<template>
  <div class="comment">
    <!-- <div class="left"> -->
    <p class="comment-user">{{ comment.user }}</p>
    <!-- </div> -->
    <p class="comment-body">{{ comment.body }}</p>
  </div>
</template>

<script>
export default {
  props: ["comment"]
};
</script>

<style scoped>
.comment {
  width: 75vw;
  display: flex;
  flex-direction: column;
  align-items: left;
  border: 1px solid white;
  margin: 10px;
}
.left {
  display: flex;
  justify-content: left;
  width: 100%;
}
.comment-body {
  padding: 10px;
  font-size: 20px;
}

.comment-user {
  padding: 5px;
  font-weight: bold;
  letter-spacing: 5px;
  text-transform: uppercase;
}
</style>