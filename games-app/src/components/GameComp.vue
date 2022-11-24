<template>
  <a :href="msg.URL">
    <img :src="require(`@/assets/${msg.imageURL}`)" :alt="msg.imageAlt">
    <div>
      <h2>{{ msg.name }}</h2>
      <p>{{ msg.description }}</p>
    </div>
  </a>
</template>

<script>

export default {
  name: "GameComp",
  props: {
    msg: Object
  }
}
</script>

<style scoped>
a {
  position: relative;
  border: 2px #492386 solid;
  padding: 0;
  color: black;
  text-decoration: none;
  aspect-ratio: 1;
  border-radius: 10px;
  overflow: hidden;
}

h2 {
  padding: 3px 0;
  margin: 0;
  text-transform: capitalize;
  font-variant: small-caps;
}

div {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  text-align: center;
  z-index: 10;
  margin: 0;
  background-color: rgba(218, 215, 215, 0.8);
  transform: translateY(100%);
  opacity: 0;
  transition: all 100ms ease-in-out;
}

img {
  top: 0;
  left: 0;
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: 5;
  filter: brightness(100%);
  transform: scale(1);
  transition: all 150ms ease-in-out;
}

a:hover div {
  transform: translateY(0%);
  opacity: 1;
  transition: all 150ms ease-in-out;
}

a:hover img {
  filter: brightness(50%);
  transform: scale(1.2);
  transition: all 300ms ease-in-out;
}
</style>
