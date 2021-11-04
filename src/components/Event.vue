<template>
  <article>
    <div class="data">
      <h3 class="name">Title</h3>
      <p class="details">some text</p>
    </div>
    <div class="countdown">
      <div class="remove_btn_wrapper">
        <button class="remove_btn">&#10060;</button>
      </div>
      <p>
        32
        <br />
        <small>days left</small>
      </p>
    </div>
  </article>
</template>

<script>
export default {
  name: "Event",
};
</script>

<style scoped>
article {
  background: lightslategrey;
  display: flex;
  align-items: center;
  border-radius: 1rem;
  margin: 1rem 0;
  padding: 0.2em 1rem;
  color: whitesmoke;
  font-weight: 300;
}

.data {
  flex: 3 1 0;
}

.countdown {
  flex: 1 1 0;
  text-align: center;
  font-size: 1.6rem;
  border-left: 1px solid;
}

.name {
  font-size: 1.8rem;
}

.details {
  font-size: 1.6rem;
}

.remove_btn {
  background: transparent;
  border: none;
  cursor: pointer;
}

.remove_btn_wrapper {
  text-align: right;
}
</style>