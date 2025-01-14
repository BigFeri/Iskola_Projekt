<template>
  <div>
    <h2>Sportok</h2>
    <!-- Táblázat -->

    <table class="table table-striped v-auto">
      <thead>
        <tr>
          <!-- rename -->
          <th scope="col">Műveletek</th>
          <th scope="col">Sport</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="dataLine in collection"
          :key="dataLine.id"
          @click="onClickTr(dataLine.id)"
          :class="{ 'table-success': selectedRowDataLineId == dataLine.id }"
        >
          <td class="text-nowrap">
            <OperationsCrud
              :dataLine="dataLine"
              @onClickDeleteButton="onClickDeleteButton"
              @onClickUpdate="onClickUpdate"
              @onClickCreate="onClickCreate"
            />
          </td>
          <!-- rename -->
          <td>{{ dataLine.sportNev }}</td>

        </tr>
      </tbody>
    </table>
    <!-- Modal -->
    <Modal
      :title="title"
      :yes="yes"
      :no="no"
      :size="size"
      @yesEvent="yesEventHandler"
    >
      <!-- yes-no (Modal) -->
      <div v-if="state == 'Delete'">
        {{ messageYesNo }}
      </div>

      <!-- Form person -->
      <ProfessionForm
        v-if="state == 'Create' || state == 'Update'"
        :dataLine="dataLine"
        @saveDataLine="saveDataLineHandler"
      />
    </Modal>
  </div>
</template>

<script>
// rename
class DataLine {
  constructor(id = null, sportNev = null) {
    this.id = id;
    this.sportNev = sportNev;
  }
}
import SportForm from "@/components/SportForm.vue";
import OperationsCrud from "@/components/OperationCrud.vue";
import * as bootstrap from "bootstrap";
import axios from "axios";
import { BASE_URL } from "@/helpers/baseUrls";
import Modal from "@/components/Modal.vue";

// import uniqid from "uniqid";
export default {
  components: {OperationsCrud, Modal, SportForm },
  mounted() {
    this.modal = new bootstrap.Modal("#modal", {
      keyboard: false,
    });
  },
  data() {
    return {
      modal: null,
      dataLine: new DataLine(this.uniqid()),
      selectedRowDataLineId: null,
      messageYesNo: null,
      state: "Read", //CRUD: Create, Read, Update, Delete
      title: null,
      yes: null,
      no: null,
      size: null,

      collection: [],
    };
  },

  mounted() {
    this.collection = this.getCollection();
    this.modal = new bootstrap.Modal("#modal", {
      keyboard: false,
    });
  },
  methods: {
    //rename
    deleteDataLineById() {
      
    },
    async getCollection() {
      const url = `${BASE_URL}/sports`;
      const response = await axios.get(url);
      this.collection = response.data.data;
      console.log(this.collection);
      
    },
    //rename
    createDataLine() {
      this.collection.push(this.dataLine);
      this.state = "Read";
    },
    //reaname
    updateDataLine() {
      const index = this.collection.findIndex((p) => p.id == this.dataLine.id);
      this.collection[index] = this.dataLine;
      this.state = "Read";
    },
    yesEventHandler() {
      if (this.state == "Delete") {
        this.deleteDataLineById();
        this.modal.hide();
      }
    },
    onClickDeleteButton(dataLine) {
      this.title = "Törlés";
      this.messageYesNo = `Valóban törölniakarod? Név: ${dataLine.name}`;
      this.yes = "Igen";
      this.no = "Nem";
      this.size = null;
      this.state = "Delete";
    },
    onClickUpdate(dataLine) {
      this.state = "Update";
      this.title = "Foglalkozás módosítása";
      this.yes = null;
      this.no = "Mégsem";
      this.size = "lg";
      // this.person = person;
      this.dataLine = { ...dataLine };
    },
    onClickCreate() {
      this.title = "Új Foglalkozás létrehozása";
      this.yes = null;
      this.no = "Mégsem";
      this.size = "lg";

      this.state = "Create";
      this.dataLine = new DataLine(this.uniqid());
    },
    onClickTr(id) {
      this.selectedRowDataLineId = id;
    },

    saveDataLineHandler(dataLine) {
      this.dataLine = dataLine;
      this.modal.hide();
      if (this.state == "Create") {
        this.createDataLine();
      } else if (this.state == "Update") {
        this.updateDataLine();
      }
    },

    uniqid(length = 10) {
      const characters =
        "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
      let result = "";
      for (let i = 0; i < length; i++) {
        const randomIndex = Math.floor(Math.random() * characters.length);
        result += characters.charAt(randomIndex);
      }
      return result;
    },
  },
  computed: {
    // collection(){
    //   //rename
    //   return this.professions
    // }
  },
};
</script>

<style>
</style>

