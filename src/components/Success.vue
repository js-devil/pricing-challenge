<template>
  <div class="jumbotron text-center">
    <h1 class="display-3">Thank You!</h1>
    <p class="lead">Your payment of ${{ price }} was successful, {{ name }}.</p>
    <hr />
    <p>
      Having trouble?
      <a href>Contact us</a>
    </p>
    <p class="lead">
      <button
        @click="$emit('close')"
        class="btn btn-primary btn-sm"
        to="/"
        role="button"
      >Continue to homepage</button>
    </p>
  </div>
</template>

<script>
export default {
  props: ["name", "price"]
};
</script>

<style>
.jumbotron {
  height: 100vh;
}
</style>