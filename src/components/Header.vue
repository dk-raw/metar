<template>
  <nav class="navbar fixed-top navbar-dark bg-dark">
    <div class="container-fluid p-2">
      <span class="navbar-brand m-auto h1 fs-3"
        ><img
          src="@/assets/512.png"
          alt=""
          width="24"
          height="24"
          class="d-inline-block align-text-center"
        />
        {{ title }}</span
      >
    </div>
  </nav>
</template>

<script>
export default {
  name: "Header",
  props: {
    title: {
      type: String,
      required: true,
    },
  },
};
</script>
