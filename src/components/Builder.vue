<template>
  <div class="builder">
    <form>
      <div class="pb-3 row g-3">
        <div class="col-md-6">
          <label for="fullname" class="form-label">Fullname</label>
          <input
            type="text"
            class="form-control"
            v-model="user.fullname"
            id="fullname"
            maxlength="25"
          />
        </div>
        <div class="col-md-6">
          <label for="position" class="form-label">Job position</label>
          <input
            type="text"
            class="form-control"
            v-model="user.position"
            id="position"
            maxlength="25"
          />
        </div>
        <div class="col-md-12">
          <label for="position" class="form-label">Profile</label>
          <div class="form-check">
            <input
              class="form-check-input"
              type="checkbox"
              value=""
              id="flexCheckDefault"
              v-model="includeProfile"
            />
            <label class="form-check-label" for="flexCheckDefault">
              Exclude profile (You can uncheck if you want profile section)
            </label>
          </div>
          <textarea
            class="form-control"
            v-model="user.profile"
            id="position"
            maxlength="350"
            :disabled="includeProfile"
            required
          >
          </textarea>
        </div>
      </div>
      <div class="education__content mb-2">
        <div
          class="pb-3 row g-3"
          v-for="(edu, index) in user.educations"
          :key="index"
        >
          <div class="col-md-6">
            <label for="university" class="form-label">University name</label>
            <input
              type="text"
              class="form-control"
              id="university"
              maxlength="40"
              v-model="edu.university"
            />
          </div>
          <div class="col-md-6">
            <label for="degree" class="form-label">Degree</label>
            <input
              type="text"
              class="form-control"
              id="degree"
              maxlength="40"
              v-model="edu.degree"
            />
          </div>
          <div class="col-md-8">
            <label for="faculty" class="form-label">Faculty</label>
            <input
              type="text"
              class="form-control"
              id="faculty"
              maxlength="40"
              v-model="edu.faculty"
            />
          </div>
          <div class="col-md-4">
            <label for="years" class="form-label">Start and end years</label>
            <input
              type="text"
              class="form-control"
              id="years"
              maxlength="40"
              v-model="edu.years"
            />
          </div>
          <button
            type="button"
            class="btn btn-outline-danger col-3 mx-auto"
            :disabled="index === 0"
            @click="deleteEducation(index)"
          >
            Delete Education -
          </button>
        </div>
        <button
          class="btn btn-outline-primary"
          @click="addEducation"
          type="button"
        >
          Add education +
        </button>
      </div>
      <div class="col-12 mt-5">
        <button type="button" @click="generate" class="btn btn-primary">
          Generate PDF
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import { jsPDF } from "jspdf";
import "../fonts/SFProDisplay-Regular-normal.js";
export default {
  data() {
    return {
      user: {
        fullname: null,
        position: null,
        profile: null,
        educations: [
          { university: null, degree: null, faculty: null, years: null },
        ],
      },
      includeProfile: true,
    };
  },
  methods: {
    addEducation() {
      const newEducation = {
        university: null,
        degree: null,
        faculty: null,
        years: null,
      };
      this.user.educations.push(newEducation);
    },
    deleteEducation(index) {
      this.user.educations.splice(index, 1);
    },

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
      doc.setFontSize(16);
      doc.text("Profile", 90, 10);
      doc.setFontSize(13);
      doc.text(this.user.profile, 90, 18, {
        align: "justify",
        maxWidth: 110,
      });
      let profileHeight =
        doc.splitTextToSize(this.user.profile, 110).length * 5 + 18;
      console.log(profileHeight);
      doc.text(this.user.education, 90, profileHeight + 5, {
        align: "justify",
        maxWidth: 110,
      });
    },
    generate() {
      const doc = new jsPDF();
      doc.setFont("SFProDisplay-Regular");
      // this.createSide(doc);
      if (!this.includeProfile) {
        this.createMain(doc);
      }

      window.open(URL.createObjectURL(doc.output("blob")));
    },
  },
};
</script>

<style>
</style>