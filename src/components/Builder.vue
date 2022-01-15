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
          maxlength="25"
        />
      </div>
      <div class="col-md-6">
        <label for="position" class="form-label">Job position</label>
        <input
          type="email"
          class="form-control"
          v-model="user.position"
          id="position"
          maxlength="25"
        />
      </div>
      <div class="col-md-12">
        <label for="position" class="form-label">Profile</label>
        <textarea
          type="email"
          class="form-control"
          v-model="user.profile"
          id="position"
          maxlength="275"
        >
        </textarea>
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
import font2 from "../fonts/SFProDisplay-Regular-normal";
export default {
  data() {
    return {
      user: {
        fullname: null,
        position: null,
        profile: null,
      },
    };
  },
  methods: {
    createSide(doc) {
      doc.setFillColor(18, 55, 124);
      doc.rect(0, 0, 80, doc.internal.pageSize.height, "F");

      doc.setFontSize(16);
      doc.setTextColor(255, 255, 255);
      doc.text(this.user.fullname, 8, 10, { maxWidth: 70 });
      doc.setFontSize(12);
      doc.text(this.user.position, 8, 15);
    },
    createMain(doc) {
      doc.setTextColor(0, 0, 0);
      doc.setFontSize(17);
      doc.text(this.user.profile, 90, 10, { align: "justify", maxWidth: 110 });
    },
    generate() {
      const doc = new jsPDF();
      doc.setFont("SFProDisplay-Regular"); // set font

      this.createSide(doc);
      this.createMain(doc);
      window.open(URL.createObjectURL(doc.output("blob")));
    },
  },
};
</script>

<style>
</style>