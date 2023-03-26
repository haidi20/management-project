<template>
  <b-row class="bg-base">
    <b-col col md="6" class="content">
      <h3>
        <b>Data Karyawan</b>
      </h3>
      <br />
      <b-row>
        <b-col cols>
          <b-form-group label="Bulan" label-for="date_filter">
            <DatePicker
              id="date_filter"
              v-model="date_filter"
              format="YYYY-MM"
              type="month"
              placeholder="pilih bulan"
            />
          </b-form-group>
        </b-col>
      </b-row>
      <br />
      <b-row>
        <b-col col md="4">
          <b-button variant="success" @click="onCreate()" size="sm">Tambah Data Karyawan</b-button>
        </b-col>
        <b-col cols>
          <b-button variant="info" @click="onRefresh()" size="sm">Perbaharui Data</b-button>
        </b-col>
      </b-row>
      <br />
      <b-row>
        <b-col cols>
          <b-table-simple hover medium caption-top responsive :bordered="true">
            <b-thead>
              <b-tr>
                <b-th></b-th>
                <b-th>NIK</b-th>
                <b-th>Nama Karyawan</b-th>
                <b-th>Gaji Pokok</b-th>
                <b-th>Alpha</b-th>
                <b-th>Ijin</b-th>
                <b-th>Hadir</b-th>
              </b-tr>
            </b-thead>
            <b-tbody>
              <b-tr v-for="(item, index) in data" :key="index">
                <b-td>
                  <b-dropdown id="form" text="Aksi" variant="info" size="sm">
                    <b-dropdown-item @click="onEdit">Edit</b-dropdown-item>
                  </b-dropdown>
                </b-td>
                <b-td>{{item.nik}}</b-td>
                <b-td>{{item.name}}</b-td>
                <b-td>{{setFormatCurrency(item.salary)}}</b-td>
                <b-td>{{item.absent}}</b-td>
                <b-td>{{item.authorization}}</b-td>
                <b-td>{{item.present}}</b-td>
              </b-tr>
            </b-tbody>
          </b-table-simple>
        </b-col>
      </b-row>
    </b-col>
    <Modals
      :form="form"
      :onSend="onSend"
      :onClearForm="onClearForm"
      :onChangeSalary="onChangeSalary"
    />
  </b-row>
</template>

<script>
import axios from "axios";
import DatePicker from "vue2-datepicker";

//partials
import Modals from "./modals.vue";

import { formatCurrency, numbersOnly } from "./utils";

const defaultForm = {
  nik_employee: null,
  name_employee: null,
  salary: null,
  salary_readable: "",
  absent: null,
  authorization: null,
  present: null,
};

export default {
  data() {
    return {
      date_filter: new Date(),
      form: {
        ...defaultForm,
      },
      data: [
        // {
        //   nik: 123,
        //   name: "rizal",
        //   salary: 3500000,
        //   absent: 1,
        //   authorization: 0, // ijin
        //   present: 21,
        // },
      ],
      // base_url: "http://localhost/BEP-ERP",
      base_url: "https://testing.bepcoal.id",
    };
  },
  components: {
    DatePicker,
    Modals,
  },
  mounted() {
    this.fetchData();
  },
  computed: {
    //
  },
  methods: {
    async fetchData() {
      const params = {
        //
      };

      try {
        await axios
          .get(`${this.base_url}/api/v1/z-karyawan/fetch-data`, {
            params: params,
          })
          .then((responses) => {
            console.info(responses);
            const data = responses.data.data;

            this.data = [...data];
          });
      } catch (error) {
        // alert(error);

        console.info(error);
      }
    },
    onRefresh() {
      this.fetchData();
    },
    onChangeSalary(value) {
      const numericValue = numbersOnly(value.toString());
      const readAble = formatCurrency(value, ".");

      this.form.salary = numericValue;
      this.form.salary_readable = readAble;
    },
    onCreate() {
      // console.info("create");
      this.$bvModal.show("form_add_employee");
    },
    onEdit() {
      this.$bvModal.show("form_update_attendance");
    },
    onSend() {
      console.info(this.form);
    },
    onSendAttendance() {
      console.info(this.form);
    },
    onClearForm() {
      this.form = { ...defaultForm };

      this.$bvModal.hide("form_add_employee");
      this.$bvModal.hide("form_update_attendance");
    },
    setFormatCurrency(value) {
      return value != null ? formatCurrency(value?.toString(), ".") : null;
    },
  },
};
</script>

<style lang="css" scoped>
.content {
  background-color: white;
  padding: 10px;
}
.bg-base {
  justify-content: center;
  background-color: #92a9bd;
  height: 100%;
}
</style>