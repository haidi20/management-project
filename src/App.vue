<template>
  <b-row class="bg-base">
    <b-col col md="6" class="content">
      <h3>
        <b>HRD - Data Karyawan</b>
      </h3>
      <br />
      <b-row>
        <b-col cols>
          <b-form-group label="Bulan" label-for="date_filter" style="display:inline-block">
            <DatePicker
              id="date_filter"
              v-model="date_filter"
              format="YYYY-MM"
              type="month"
              placeholder="pilih bulan"
            />
          </b-form-group>
          <b-button
            variant="info"
            @click="onRefresh()"
            size="sm"
            style="margin-left: 10px"
          >Perbaharui Data</b-button>
          <b-button
            variant="success"
            @click="onCreate()"
            size="sm"
            style="margin-left: 10px"
          >Tambah Data Karyawan</b-button>
        </b-col>
      </b-row>
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
                    <b-dropdown-item @click="onShowSalary(item)">Total Gaji</b-dropdown-item>
                    <b-dropdown-item @click="onEdit(item)">Ubah</b-dropdown-item>
                    <b-dropdown-item @click="onDelete(item.id)">Hapus</b-dropdown-item>
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
      :onSendAttendance="onSendAttendance"
    />
  </b-row>
</template>

<script>
import axios from "axios";
import moment from "moment";
import DatePicker from "vue2-datepicker";

//partials
import Modals from "./modals.vue";

import { formatCurrency, numbersOnly } from "./utils";

const defaultForm = {
  nik: null,
  name: null,
  z_employee_id: null,
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
  watch: {
    date_filter(newValue, oldValue) {
      // console.info(newValue);
    },
  },
  methods: {
    async fetchData() {
      const params = {
        date_filter: moment(this.date_filter).format("Y-MM-DD"),
      };

      // console.info(params);

      try {
        await axios
          .get(`${this.base_url}/api/v1/z-karyawan/fetch-data`, {
            params: params,
          })
          .then((responses) => {
            // console.info(responses);
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
      this.onClearForm();
      this.$bvModal.show("form_add_employee");
    },
    onEdit(item) {
      this.form = {
        ...this.form,
        ...item,
      };
      this.form.name_employee = item.name;
      this.form.z_employee_id = item.id;
      this.$bvModal.show("form_update_attendance");
    },
    async onSend() {
      // return false;

      this.is_loading = true;

      await axios
        .post(`${this.base_url}/api/v1/z-karyawan/store`, {
          ...this.form,
        })
        .then((responses) => {
          this.is_loading = false;
          const data = responses.data;

          if (data.success == true) {
            this.fetchData();
            this.$bvModal.hide("form_add_employee");
            this.onClearForm();
          }
        })
        .catch((err) => {
          console.info(err);
          this.is_loading = false;
        });
    },
    async onSendAttendance() {
      // console.info(this.form);
      // return false;

      this.is_loading = true;

      await axios
        .post(`${this.base_url}/api/v1/z-karyawan/store-attendance`, {
          ...this.form,
          date: moment(this.date_filter).format("Y-MM-DD"),
        })
        .then((responses) => {
          // console.info(responses);
          this.is_loading = false;
          const data = responses.data;

          if (data.success == true) {
            this.fetchData();
            this.$bvModal.hide("form_update_attendance");
            this.onClearForm();
          }
        })
        .catch((err) => {
          console.info(err);
          this.is_loading = false;
        });
    },
    async onDelete(id) {
      // return false;

      this.is_loading = true;

      await axios
        .post(`${this.base_url}/api/v1/z-karyawan/delete`, {
          id: id,
        })
        .then((responses) => {
          this.is_loading = false;
          const data = responses.data;

          this.fetchData();
        })
        .catch((err) => {
          console.info(err);
          this.is_loading = false;
        });
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