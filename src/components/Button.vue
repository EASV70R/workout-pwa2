<template>
  <div>
    <button @click="handleClick" :class="['cta-btn', filled ? 'filled' : '']">{{text}}</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ctaBtn: ""
    };
  },
  mounted() {
    // console.log(this.filled);
  },
  props: {
    text: String,
    filled: Boolean
  },
  methods: {
    handleClick() {
      this.$emit("click");
    }
  }
};
</script>

<style scoped>
.cta-btn {
  margin-top: 50px;
  border: 2px solid white;
  color: var(--main-text-color);
  background: none;
  font-size: 25px;
  padding: 15px;
  cursor: pointer;
  position: relative;
  transition: 0.09s ease-in;
  border-radius: 4px;
  z-index: 2;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-family: "Rubik", sans-serif;
}

.cta-btn:focus {
  outline: none;
}

.filled {
  color: var(--main-bg-color);
  background-color: white;
}

.cta-btn.filled:hover {
  color: white;
}

.cta-btn:hover {
  color: var(--main-bg-color);
}

.cta-btn::before {
  content: "";
  position: absolute;
  background: white;
  color: var(--main-bg-color);
  bottom: 0;
  left: 0;
  right: 0;
  top: 100%;
  z-index: -1;
  transition: top 0.09s ease-in;
}

.cta-btn.filled::before {
  background: var(--main-bg-color);
  color: white;
  bottom: 100%;
  top: 0;
  transition: bottom 0.09s ease-in;
}

.cta-btn.filled:hover::before {
  bottom: 0%;
}

.cta-btn:hover::before {
  top: 0;
}
</style>