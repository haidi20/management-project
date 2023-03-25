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
        <b-col cols>
          <b-button variant="success" @click="onCreate()" size="sm">Tambah Data Karyawan</b-button>
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
                  <b-dropdown id="form" text="Aksi" variant="success" size="sm">
                    <b-dropdown-item @click="onEdit">Edit</b-dropdown-item>
                  </b-dropdown>
                </b-td>
                <b-td>{{item.nik}}</b-td>
                <b-td>{{item.name}}</b-td>
                <b-td>{{item.salary}}</b-td>
                <b-td>{{item.absent}}</b-td>
                <b-td>{{item.authorization}}</b-td>
                <b-td>{{item.present}}</b-td>
              </b-tr>
            </b-tbody>
          </b-table-simple>
        </b-col>
      </b-row>
    </b-col>
    <!-- <b-col col md="3"></b-col> -->
    <b-modal id="form" ref="form" size="lg" hide-footer>
      <b-row>
        <b-col col md="4">
          <b-form-group label="Nik Karyawan" label-for class>
            <b-form-input v-model="form.nik_employee" id="nik_employee" name="nik_employee"></b-form-input>
          </b-form-group>
        </b-col>
        <b-col col md="4">
          <b-form-group label="Nama Karyawan" label-for class>
            <b-form-input v-model="form.name_employee" id="name_employee" name="name_employee"></b-form-input>
          </b-form-group>
        </b-col>
        <b-col col md="4">
          <b-form-group label="Gaji Pokok" label-for class>
            <b-form-input
              v-model="form.salary_readable"
              @input="value => onChangeSalary(value)"
              id="salary"
              name="salary"
            ></b-form-input>
          </b-form-group>
        </b-col>
      </b-row>
      <b-row>
        <b-col col md="4">
          <b-form-group label="Alpa" label-for class>
            <b-form-input v-model="form.absent" id="absent" name="absent"></b-form-input>
          </b-form-group>
        </b-col>
        <b-col col md="4">
          <b-form-group label="Ijin" label-for class>
            <b-form-input v-model="form.authorization" id="authorization" name="authorization"></b-form-input>
          </b-form-group>
        </b-col>
        <b-col col md="4">
          <b-form-group label="Hadir" label-for class>
            <b-form-input v-model="form.present" id="present" name="present"></b-form-input>
          </b-form-group>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols>
          <b-button class="float-right ml-4" variant="success" @click="onSend()">Simpan</b-button>
          <b-button class="float-right" variant="danger" @click="onClearForm()">Batal</b-button>
        </b-col>
      </b-row>
    </b-modal>
  </b-row>
</template>

<script>
import DatePicker from "vue2-datepicker";

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
        {
          nik: 123,
          name: "rizal",
          salary: 3500000,
          absent: 1,
          authorization: 0, // ijin
          present: 21,
        },
      ],
    };
  },
  components: {
    DatePicker,
  },
  computed: {
    //
  },
  methods: {
    onChangeSalary(value) {
      const numericValue = numbersOnly(value.toString());
      const readAble = formatCurrency(value, ".");

      this.form.salary = numericValue;
      this.form.salary_readable = readAble;
    },
    onCreate() {
      // console.info("create");
      this.$bvModal.show("form");
    },
    onEdit() {
      //
    },
    onSend() {
      console.info(this.form);
    },
    onClearForm() {
      this.form = { ...defaultForm };
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