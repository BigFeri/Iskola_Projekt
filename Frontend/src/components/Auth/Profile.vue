<template>
  <div class="container">
    <div class="row my-5">
      <div class="col-md-6 mx-auto">
        <div class="card">
          <h5 class="card-header text-center">Profil módosítás</h5>
          <div class="card-body">
            <form @submit.prevent="updateProfile">
              <div class="form-group mb-3">
                <label for="name">Name</label>
                <input
                  type="text"
                  id="name"
                  v-model="user.name"
                  placeholder="Name"
                  class="form-control"
                />
              </div>
              <div class="form-group mb-3">
                <label for="email">Email</label>
                <input
                  type="email"
                  id="email"
                  v-model="user.email"
                  placeholder="Email"
                  class="form-control"
                />
              </div>
              <div class="form-group mb-3">
                <label for="currentPassword">Jelenlegi jelszó</label>
                <input
                  type="password"
                  id="currentPassword"
                  v-model="user.currentPassword"
                  placeholder="Jelenlegi jelszó"
                  class="form-control"
                />
              </div>
              <div class="form-group mb-3">
                <label for="newPassword">Új jelszó</label>
                <input
                  type="password"
                  id="newPassword"
                  v-model="user.newPassword"
                  placeholder="Új jelszó"
                  class="form-control"
                />
              </div>

              <div class="d-flex align-items-center">
                <button type="submit" class="btn btn-primary me-4">
                  Frissítés
                </button>

                <div
                  class="spinner-border m-0 p-0"
                  role="status"
                  v-if="loading"
                >
                  <span class="visually-hidden m-0">Loading...</span>
                </div>

                <span v-if="errorMessage" class="text-danger">{{
                  errorMessage
                }}</span>
                <span v-if="successMessage" class="text-success">{{
                  successMessage
                }}</span>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { useAuthStore } from "../../stores/useAuthStore.js";
import axios from "axios";
import { BASE_URL } from "../../helpers/baseUrls";

export default {
  data() {
    return {
      user: {
        name: "",
        email: "",
        currentPassword: "",
        newPassword: "",
      },
      loading: false,
      errorMessage: null,
      successMessage: null,
      store: useAuthStore(),
    };
  },
  mounted() {
    this.user.name = this.store.user || "";
    this.user.email = this.store.email || "";
  },
  methods: {
    async updateProfile() {
      this.loading = true;
      this.errorMessage = null;
      this.successMessage = null;

      if (this.user.currentPassword === this.user.newPassword) {
        this.loading = false;
        this.errorMessage = "A régi jelszó és az új jelszó nem lehet ugyanaz!";
        return;
      }

      const token = this.store.token;
      const url = `${BASE_URL}/users/${this.store.id}`;
      const headers = {
        Accept: "application/json",
        "Content-Type": "application/json",
        Authorization: `Bearer ${token}`,
      };

      const dataToUpdate = {
        name: this.user.name,
        email: this.user.email,
        currentPassword: this.user.currentPassword,
        newPassword: this.user.newPassword,
      };

      // try {
        // const response = await axios.patch(url, dataToUpdate, { headers });
        // this.successMessage = "Profil megváltoztatva!";

        // this.store.setUser(response.data.data.name);
        // this.store.setEmail(response.data.data.email);

      //   this.loading = false;
      // } catch (error) {
      //   this.loading = false;
      //   if (error.response && error.response.data) {
      //     this.errorMessage = error.response.data.message;
      //   } else {
      //     this.errorMessage = "Hiba!";
      //   }
      // }

      try {
        const response = await axios.patch(url, dataToUpdate, headers );
        this.store.setUser(response.data.data.name);
        this.store.setEmail(response.data.data.email);
        if (response.data.message === "ok") {
          this.errorMessage = "Sikeres módosítás!";
          setTimeout(() => {
            this.$router.push("/");
          }, 3000);
        } else {
          this.errorMessage = `${response.data.message}`;
        }
      } catch (error) {
        if (error.response && error.response.data) {
          this.errorMessage = `${response.data.message}`;
        } else {
          this.errorMessage = "Sikertelen módosítás! Kapcsolódási hiba.";
        }

        this.store.clearStoredData();
      }
    },
  },
};
</script>

<style scoped>
</style>
