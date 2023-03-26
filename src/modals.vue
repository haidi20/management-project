<template>
  <div>
    <b-modal
      id="form_add_employee"
      ref="form_add_employee"
      title="Data Karyawan"
      size="lg"
      hide-footer
    >
      <b-row>
        <b-col col md="4">
          <b-form-group label="Nik Karyawan" label-for class>
            <b-form-input v-model="form.nik" id="nik" name="nik"></b-form-input>
          </b-form-group>
        </b-col>
        <b-col col md="4">
          <b-form-group label="Nama Karyawan" label-for class>
            <b-form-input v-model="form.name" id="name" name="name"></b-form-input>
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
        <b-col cols>
          <b-button class="float-right ml-4" variant="success" @click="onSend()">Simpan</b-button>
          <b-button class="float-right" variant="danger" @click="onClearForm()">Batal</b-button>
        </b-col>
      </b-row>
    </b-modal>
    <b-modal
      id="form_update_attendance"
      ref="form_update_attendance"
      title="Perbaharui Kehadiran Karyawan"
      size="lg"
      hide-footer
    >
      <b-row>
        <b-col col md="4">Nama Karyawan : {{form.name_employee}}</b-col>
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
          <b-button class="float-right ml-4" variant="success" @click="onSendAttendance()">Simpan</b-button>
          <b-button class="float-right" variant="danger" @click="onClearForm()">Batal</b-button>
        </b-col>
      </b-row>
    </b-modal>
  </div>
</template>

<script>
export default {
  props: {
    form: Object,
    onSend: Function,
    onClearForm: Function,
    onChangeSalary: Function,
    onSendAttendance: Function,
  },
};
</script>

<style lang="scss" scoped>
</style>