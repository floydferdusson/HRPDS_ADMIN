<template>
  <div>
    <q-card>
      <q-card-section style="max-height: 50vh" class="scroll">
        <div class="text-h6">
          WORK EXPERIENCE
         <!--  <q-btn label="Add" @click="secondDialog = true"></q-btn> -->
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
          <div class="text-h6">Add Work Details</div>
        </q-card-section>
        <q-separator />
        <q-card-section>
          <q-form ref="experience" lazy-rules>
            <div class="row">
              <div class="col">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="InclusiveDateFrom"
                  label="Inclusive Date From"
                  dense
                  class="q-pa-sm"
                  type="date"
                />
              </div>
              <div class="col">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="InclusiveDateTo"
                  label="Inclusive Date To"
                  dense
                  class="q-pa-sm"
                  type="date"
                />
              </div>
            </div>
            <div class="row">
              <div class="col">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="PositionTitle"
                  label="Position Title"
                  dense
                  class="q-pa-sm text-uppercase"
                />
              </div>
              <div class="col">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="Department"
                  label="Department"
                  dense
                  class="q-pa-sm text-uppercase"
                />
              </div>
            </div>
            <div class="row">
              <div class="col">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="MonthlySalary"
                  label="Monthly Salary"
                  dense
                  class="q-pa-sm"
                />
              </div>
              <div class="col">
                <q-input
                  filled
                  v-model="SalaryGrade"
                  label="Salary Grade"
                  dense
                  class="q-pa-sm"
                />
              </div>
            </div>
            <div class="row">
              <div class="col">
                <q-input
                  lazy-rules
                  :rules="inputRules"
                  filled
                  v-model="StatusofAppointment"
                  label="Status of Appointment"
                  dense
                  class="q-pa-sm text-uppercase"
                />
              </div>
              <div class="col">
                <div class="q-gutter-sm q-mt-sm q-ml-sm">
                  <label for="">Government Service</label>
                  <br />
                  <q-radio
                    v-model="GovtService"
                    checked-icon="task_alt"
                    unchecked-icon="panorama_fish_eye"
                    val="yes"
                    dense
                    label="Yes"
                  />
                  <q-radio
                    v-model="GovtService"
                    checked-icon="task_alt"
                    unchecked-icon="panorama_fish_eye"
                    val="no"
                    dense
                    label="No"
                  />
                </div>
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
            label="Request update"
            color="secondary"
            size="md"
            v-close-popup
            @click="addWorkexperience()"
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
import { useLoginStore } from "../stores/LoginStore";
import { useUserInfoStore } from "../stores/AdditionalStore";
export default {
  data() {
    return {
      ControlNo: "",
      inputRules: [(v) => !!v || "Field is required"],
      small: false,
      smalltitle: "",
      smallmessage: "",
      InclusiveDateFrom: "",
      InclusiveDateTo: "",
      PositionTitle: "",
      Department: "",
      MonthlySalary: "",
      SalaryGrade: "",
      StatusofAppointment: "",
      GovtService: "no",
      personal: [],
      secondDialog: false,
      model: ref(null),
      name: "",
      dateofbirth: "",
      columns: [
        {
          name: "InclusiveDateFrom",
          required: true,
          label: "From",
          align: "left",
          field: "wfrom",
          format: (val) => `${val}`,
          sortable: true,
        },
        {
          name: "InclusiveDateTo",
          align: "center",
          label: "To",
          field: "wto",
          sortable: true,
        },
        {
          name: "PositionTitle",
          align: "center",
          label: "PositionTitle",
          field: "wposition",
          sortable: true,
        },
        {
          name: "Department",
          align: "center",
          label: "Department",
          field: "wcompany",
          sortable: true,
        },
        {
          name: "MonthlySalary",
          align: "center",
          label: "Monthly Salary",
          field: "wsalary",
          format: (val) => {
            const numVal = Number(val);
            if (!isNaN(numVal)) {
              return numVal.toLocaleString("en-US", {
                style: "currency",
                currency: "PHP",
              });
            }
            return val;
          },
          sortable: true,
        },
        {
          name: "SalaryGrade",
          align: "center",
          label: "Salary Grade",
          field: "wgrade",
          sortable: true,
        },
        {
          name: "StatusofAppointment",
          align: "center",
          label: "Status of Appointment",
          field: "status",
          sortable: true,
        },
        {
          name: "GovtService",
          align: "center",
          label: "Gov't Service",
          field: "wgov",
          sortable: true,
        },
      ],
      rows: [
        {
          InclusiveDateFrom: "2",
          InclusiveDateTo: "s",
          PositionTitle: "s",
          Department: "s",
          MonthlySalary: "s",
          SalaryGrade: "s",
          StatusofAppointment: "s",
          GovtService: "s",
        },
      ],
    };
  },
  created() {
    this.personal = [];
    const userstore = useLoginStore();
    // this.personal=userstore.userexperience.map((item)=>({...item}));
    this.personal = userstore.userexperience;
    this.ControlNo = userstore.controlno;
    const userinfostore = useUserInfoStore();

    let data = new FormData();
    data.append("controlno", userstore.controlno);
    userinfostore.getworkexp(data).then((res) => {
      // console.log("service record=", userinfostore.userexperience);
      // console.log("experience=", this.personal);
      let servicerecord = userinfostore.workexp;

      servicerecord.forEach((item) => {
        if (
          !this.personal.some(
            (existingItem) => existingItem.wposition == item.wposition
          )
        ) {
          this.personal.push(item);
        }
      });
      // console.log("experience=",this.personal)
    });
    // console.log("experience=",this.personal)
  },
  beforeUnmount() {
    this.resetall();
  },
  methods: {
    resetall() {
      this.personal = [];
    },
    async addWorkexperience() {
      const valid = await this.$refs.experience.validate();
      if (!this.fileimage) {
        this.smalltitle = "Error!";
        this.smallmessage =
          "You need to upload a suppporting document in image format.";
        this.small = true;
      }
      if (valid) {
        let Form = new FormData();

        Form.append("ControlNo", this.ControlNo);
        Form.append("wfrom", this.InclusiveDateFrom);
        Form.append("wto", this.InclusiveDateTo);
        Form.append("wposition", this.PositionTitle);
        Form.append("wcompany", this.Department);
        Form.append("wsalary", this.MonthlySalary);
        Form.append("status", this.StatusofAppointment);
        Form.append(
          "wgrade",
          isNaN(this.SalaryGrade) ? this.SalaryGrade : "N/A"
        );
        Form.append("wgov", this.GovtService);

        Form.append("file", this.fileimage);
        const userstore = useUserInfoStore();
        userstore.saveWorkExperience(Form).then((res) => {
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

    // additionalinfo.civilservice().then((res) => {
    //   eligibilityoptions.value = [...additionalinfo.civilservice];
    //   origdata.value = [...additionalinfo.civilservice];
    // });
    // const filterFn = (val, update, abort) => {
    //   const needle = (val || "").toLowerCase();
    //   const filteredItems = origdata.value.filter((item) =>
    //     item.Descriptions.toLowerCase().includes(needle)
    //   );

    //   update(() => {
    //     eligibilityoptions.value = filteredItems;
    //   });
    // };
    return {
      file,
      fileimage,
      fileimagesrc,
      handleUpload,
      handleUploadBtnClick,
    };
  },
};
</script>
