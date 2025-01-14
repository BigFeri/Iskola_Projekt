<template>
  <div>
    <p>{{ dataLine }}</p>
    <form class="row g-4 needs-validation was-validated" novalidate>
      <!-- Name -->
      <div class="col-md-8">
        <label for="sportNev" class="form-label">Sport Név:</label>
        <input
          type="text"
          class="form-control"
          id="sportNev"
          required
          v-model="dataLine.name"
        />
        <div class="valid-feedback">&nbsp;</div>
        <div class="invalid-feedback">Név kitöltése kötelező</div>
      </div>
    </form>
  </div>
</template>
  
  <script>
import dateFormat, { masks } from "dateformat";
export default {
  props: ["dataLine"],
  emits: ["saveDataLine"],
  mounted() {
    const forms = document.querySelectorAll(".needs-validation");

    // Loop over them and prevent submission
    Array.from(forms).forEach((form) => {
      form.addEventListener(
        "submit",
        (event) => {
          event.preventDefault();
          event.stopPropagation();
          if (form.checkValidity()) {
            this.onClickSubmit();
          }
        },
        false
      );
    });
  },
  methods: {
    onClickSubmit() {
      console.log("Mentés");
      this.$emit("saveDataLine", this.dataLine);
    },
  },
  computed: {},
};
</script>
  
  <style>
</style>