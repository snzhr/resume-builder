<template>
  <div class="builder">
    <form class="row g-2">
      <div class="col-md-6">
        <label for="fullname" class="form-label">Fullname</label>
        <input
          type="email"
          class="form-control"
          v-model="user.fullname"
          id="fullname"
        />
      </div>
      <div class="col-md-6">
        <label for="position" class="form-label">Job position</label>
        <input
          type="email"
          class="form-control"
          v-model="user.position"
          id="position"
        />
      </div>
      <div class="col-12">
        <button type="button" @click="generate" class="btn btn-primary">
          Generate PDF
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import { jsPDF } from "jspdf";
export default {
  data() {
    return {
      user: {
        fullname: "",
        position: "",
      },
    };
  },
  methods: {
    generate() {
      const doc = new jsPDF();
    },
  },
};
</script>

<style>
</style>