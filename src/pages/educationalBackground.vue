<template>
  <div>
    <q-card>
      <q-card-section style="max-height: 50vh" class="scroll">
        <div class="text-h6">
          EDUCATIONAL BACKGROUND
       <!--    <q-btn label="Add" @click="addnew()"></q-btn> -->
        </div>
      </q-card-section>
      <q-table
        class="my-sticky-header-table"
        flat
        bordered
        title=""
        dense
        :rows="personal"
        :columns="columns"
        row-key="id"
      ></q-table>
    </q-card>
    <q-dialog
      v-model="secondDialog"
      persistent
      transition-show="scale"
      transition-hide="scale"
    >
      <q-card class="" style="width: 500px">
        <q-card-section>
          <div class="text-h6">Add Details</div>
        </q-card-section>
        <q-separator />
        <q-card-section>
          <q-form ref="educational" lazy-rules>
            <div class="row">
              <div class="col-md-4 col-sm-4 col-xs-12">
                <q-select
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="level"
                  label="Educational Level"
                  dense
                  class="q-pa-sm"
                  :options="leveloptions"
                />
              </div>
              <div class="col-md-8 col-sm-8 col-xs-12">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="name"
                  label="Name of School"
                  dense
                  class="q-pa-sm"
                />
              </div>
              <div class="col-md-12 col-sm-12 col-xs-12">
                <q-select
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="degree"
                  use-input
                  hide-selected
                  fill-input
                  input-debounce="0"
                  label="Basic Education/Degree/Course"
                  dense
                  class="q-pa-sm text-uppercase"
                  :options="degreeoption"
                  :option-label="(degree) => degree.Descriptions"
                  @filter="filterFn"
                />
              </div>
              <div class="col-md-6 col-sm-6 col-xs-12">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="PeriodofAttendanceFrom"
                  label="Period of Attendance From"
                  dense
                  class="q-pa-sm"
                  type="date"
                />
              </div>
              <div class="col-md-6 col-sm-6 col-xs-12">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="PeriodofAttendanceTo"
                  label="Period of Attendance To"
                  dense
                  class="q-pa-sm"
                  type="date"
                />
              </div>
            </div>
            <div class="row">
              <label for="Graduated" class="q-ml-lg">Graduated:</label>
              <q-radio v-model="Graduated" val="YES" label="Yes" inline />
              <q-radio v-model="Graduated" val="NO" label="No" inline />
              <div class="col-md-6 col-sm-6 col-xs-12" v-if="Graduated == 'NO'">
                <q-input
                  filled
                  v-model="Unit"
                  label="Highest Level / Units Earned"
                  dense
                  class="q-pa-sm"
                />
              </div>
              <!-- <div class="col-md-6 col-sm-6 col-xs-12" v-if="Graduated == 'YES'">
              <q-input
                filled
                v-model="Unit"
                label="Year Graduated"
                dense
                class="q-pa-sm"
                type="number"
              />
            </div> -->

              <div class="col-md-12 col-sm-12 col-xs-12">
                <q-input
                  filled
                  v-model="Scholarship"
                  label="Scholarship / Academic Honors Received"
                  dense
                  class="q-pa-sm"
                />
              </div>
            </div>
            <div class="row">
              <div class="col">
                <q-file
                  lazy-rules
                  :rules="inputRules"
                  style="display: none"
                  v-model="fileimage"
                  @update:model-value="handleUpload"
                  accept=".jpg, image/*"
                  ref="file"
                ></q-file>
                <div class="column items-center">
                  <q-btn
                    type="button"
                    label="Upload Supporting Document"
                    @click="handleUploadBtnClick"
                  ></q-btn>
                </div>
              </div>
            </div>
            <div class="q-pt-sm">
              <q-img
                lazy-rules
                :rules="inputRules"
                :src="fileimagesrc"
                spinner-color="white"
              ></q-img>
            </div>
          </q-form>
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="Cancel" color="primary" v-close-popup size="md" />
          <q-btn
            label="Request for Update"
            color="secondary"
            size="md"
            @click="addEducation()"
          />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <q-dialog v-model="small">
      <q-card style="width: 300px">
        <q-card-section>
          <div class="text-h6">{{ smalltitle }}!</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          {{ smallmessage }}
        </q-card-section>

        <q-card-actions align="right" class="bg-white text-teal">
          <q-btn flat label="OK" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import { ref } from "vue";
import { useLoginStore } from "src/stores/LoginStore";
import { useUserInfoStore } from "src/stores/AdditionalStore";

export default {
  data() {
    return {
      // education:
      smalltitle: "",
      smallmessage: "",
      small: false,
      inputRules: [(v) => !!v || "Field is required"],
      Scholarship: "",
      Graduated: "",
      ControlNo: "",
      addEducationalBackground: [],
      degree: ref(null),
      PeriodofAttendanceFrom: "",
      PeriodofAttendanceTo: "",
      Unit: "",
      personal: [],
      secondDialog: false,
      model: ref(null),
      name: "",
      dateofbirth: "",
      level: "",
      leveloptions: [
        "ELEMENTARY",
        "SECONDARY",
        "VOCATIONAL/TRADE COURSE",
        "COLLEGE",
        "GRADUATE STUDIES",
      ],
      columns: [
        {
          name: "Level",
          required: true,
          label: "Level",
          align: "left",
          field: "Education",

          sortable: true,
        },
        {
          name: "NameofSchool",
          align: "center",
          label: "Name of School",
          field: "School",
          sortable: true,
        },
        {
          name: "Degree",
          align: "center",
          label: "Basic Education/Degree/Course",
          field: "Degree",
          sortable: true,
        },
        {
          name: "PeriodofAttendanceFrom",
          align: "center",
          label: "From",
          field: "From",
          sortable: true,
        },
        {
          name: "PeriodofAttendanceTo",
          align: "center",
          label: "To",
          field: "To",
          sortable: true,
        },
        {
          name: "Unit",
          align: "center",
          label: "Highest Level/Units Earned",
          field: "NumUnits",
          sortable: true,
        },
        {
          name: "YearGraduated",
          align: "center",
          label: "Year Graduated",
          field: "yeargraduated",
          sortable: true,
        },
        {
          name: "Scholarship",
          align: "center",
          label: "Scholarship/Academic Honors Received",
          field: "Honors",
          sortable: true,
        },
      ],
      rows: [
        {
          Level: "2",
          NameofSchool: "s",
          Degree: "s",
          PeriodofAttendanceFrom: "s",
          PeriodofAttendanceTo: "s",
          Unit: "s",
          YearGraduated: "s",
          Scholarship: "s",
        },
      ],
    };
  },
  created() {
    const userstore = useLoginStore();
    this.personal = userstore.usereducation;
    for (let i = 0; i < this.personal.length; i++) {
      this.personal[i].From = this.personal[i].DateAttend.slice(0, 4);
      this.personal[i].To = this.personal[i].DateAttend.slice(6);
      if (this.personal[i].Graduated == "YES") {
        this.personal[i].yeargraduated = this.personal[i].To;
        this.personal[i].NumUnits = "";
      } else {
        this.personal.yeargraduated = "";
      }
    }
    console.log("education=", this.personal);
    this.ControlNo = userstore.controlno;
  },
  methods: {
    addnew() {
      this.secondDialog = true;
      // const additionalinfo = useUserInfoStore();

      // additionalinfo.educationdegree().then((res) => {
      //   this.degreeoption = [...additionalinfo.degree];
      //   console.log("degreeoption=",this.degreeoption)
      // });
    },
    async addEducation() {
      const valid = await this.$refs.educational.validate();
      if (!this.fileimage) {
        this.smalltitle = "Error!";
        this.smallmessage =
          "You need to upload a suppporting document in image format.";
        this.small = true;
      }
      if (valid) {
        let Form = new FormData();

        Form.append("Codes", this.degree.codes);
        Form.append("ControlNo", this.ControlNo);
        Form.append(
          "DateAttend",
          this.PeriodofAttendanceFrom + " - " + this.PeriodofAttendanceTo
        );
        Form.append("Degree", this.degree.Descriptions);
        Form.append("Education", this.level);
        Form.append("From", this.PeriodofAttendanceFrom);
        Form.append("Graduated", this.Graduated);
        Form.append("Honors", this.Scholarship);
        Form.append("NumUnits", isNaN(this.Unit) ? this.Unit : "");
        Form.append("Orders", this.personal[0].length + 1);
        Form.append("School", this.name);
        Form.append("To", this.PeriodofAttendanceTo);
        Form.append("YearLevel", isNaN(this.Unit) ? "" : this.Unit);
        Form.append(
          "yeargraduated",
          this.Graduated == "YES" ? this.PeriodofAttendanceTo : ""
        );
        Form.append("file", this.fileimage);
        const userstore = useUserInfoStore();
        userstore.saveeducation(Form).then((res) => {
          console.log("res=", res);
          if (res == 1) {
            this.secondDialog = false;
            this.smalltitle = "Requested!";
            this.smallmessage =
              "The request for update has been sent to the HR.";
            this.small = true;
          } else if (res == 2) {
            this.secondDialog = false;
            this.smalltitle = "Duplicate Entry!";
            this.smallmessage =
              "The request for update has already been sent to the HR, Please check the Request history for more information.";
            this.small = true;
          } else {
            this.secondDialog = false;
            this.smalltitle = "Error!";
            this.smallmessage =
              "Please contact the SPM Team on local number 508";
            this.small = true;
          }
        });
      }
    },
  },
  setup() {
    const degreeoption = ref([]);
    const origdata = ref([]);
    const additionalinfo = useUserInfoStore();

    const file = ref(null);
    const fileimage = ref(null);
    const fileimagesrc = ref("");
    const handleUpload = () => {
      if (fileimage.value) {
        fileimagesrc.value = URL.createObjectURL(fileimage.value);
      }
    };
    const handleUploadBtnClick = () => {
      file.value.pickFiles();
    };

    additionalinfo.educationdegree().then((res) => {
      degreeoption.value = [...additionalinfo.degree];
      origdata.value = [...additionalinfo.degree];
    });

    const filterFn = (val, update, abort) => {
      const needle = (val || "").toLowerCase();
      const filteredItems = origdata.value.filter((item) =>
        item.Descriptions.toLowerCase().includes(needle)
      );

      update(() => {
        degreeoption.value = filteredItems;
      });
    };

    return {
      degreeoption,
      filterFn,
      file,
      fileimage,
      fileimagesrc,
      handleUpload,
      handleUploadBtnClick,
    };
  },
};
</script>
