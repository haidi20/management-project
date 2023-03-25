<template>
  <b-row class="bg-base">
    <b-col col offset-md="3" md="5" class="content">
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
          <b-button variant="success" @click="onCreate()" size="sm">Buat</b-button>
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
    <b-col col md="3"></b-col>
    <b-modal id="form" ref="form" size="xl" hide-footer>
      <b-row>
        <b-col col md="12">form</b-col>
      </b-row>
    </b-modal>
  </b-row>
</template>

<script>
import DatePicker from "vue2-datepicker";

export default {
  data() {
    return {
      date_filter: new Date(),
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
  methods: {
    onCreate() {
      // console.info("create");
      this.$bvModal.show("form");
    },
    onEdit() {
      //
    },
  },
};
</script>

<style lang="scss" scoped>
html,
body {
  height: 100%;
}
.content {
  background-color: white;
  padding: 10px;
}
.bg-base {
  background-color: #92a9bd;
  height: 100%;
}
</style>