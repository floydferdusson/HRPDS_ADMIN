<template>
  <div>
    <div>
      <h5 class="q-ml-lg">HRPDS ANNOUNCEMENT</h5>
      <div class="column items-center" style="">
        <div class="col" style="width: 90%">
          <q-btn
            label="CREATE"
            @click="Rowclick"
            class="q-mb-sm q-mr-md text-green"
          />
          <q-card class="q-pa-sm" style="text-align: center">
            <q-table
              class="my-sticky-header-table"
              flat
              bordered
              title="Announcement List"
              dense
              :rows="rows"
              :columns="columns"
              row-key="id"
              :rows-per-page-options="[0]"
            >
              <template v-slot:body-cell-actions="{ row }">
                <div class="actionsbtn">
                  <q-btn
                    icon="edit"
                    flat
                    round
                    color="secondary"
                    @click="editItem(row)"
                  >
                  </q-btn>
                  <q-btn
                    icon="delete"
                    flat
                    round
                    color="deep-orange"
                    @click="deleteItem(row)"
                  >
                  </q-btn>
                </div>
              </template>
            </q-table>
          </q-card>

          <q-dialog v-model="CreationDialog" persistent>
            <q-card style="width: 40%; height: auto">
              <q-card-section>
                <div class="text-h6">Announcement Details</div>

                <q-card-section style="height: auto" class="scroll">
                  <q-form>
                    <div class="row">
                      <div class="col-12">
                        <q-input
                          type="textarea"
                          filled
                          v-model="announcementname"
                          label="Announcement Name"
                          dense
                          class="q-pa-sm"
                        />
                      </div>
                    </div>
                  </q-form>
                </q-card-section>

                <div class="row">
                  <div class="col-12 col-sm-6 col-md-3 col-lg-7 q-ml-xl">
                    <div class="q-pa-sm q-ml-xl">
                      <label>
                        <img
                          v-if="imageUrl"
                          :src="imageUrl"
                          alt="Selected Image"
                          style="
                            cursor: pointer;
                            max-width: 200%;
                            max-height: 195px;
                          "
                          @click="openFileInput"
                        />
                        <div v-else class="placeholder" style="cursor: pointer">
                          Click to add an image
                        </div>
                        <input
                          ref="fileInput"
                          type="file"
                          style="display: none"
                          @change="handleFileChange"
                        />
                      </label>
                    </div>
                  </div>
                </div>
              </q-card-section>

              <q-separator />

              <q-separator />

              <q-card-actions align="right">
                <q-btn
                  flat
                  label="Cancel"
                  color="primary"
                  v-close-popup
                  @click="cancel"
                />
                <q-btn label="Save" color="green" v-close-popup @click="save" />
              </q-card-actions>
            </q-card>
          </q-dialog>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      CreationDialog: false,
      imageUrl: null,
      columns: [
        {
          name: "announcementname",
          align: "left",
          label: "ANNOUNCEMENT NAME",
          field: "test",
          sortable: true,
        },
        {
          name: "actions",
          label: "ACTIONS",
          field: "actions",
          align: "center",
        },
      ],
      rows: [
        {
          test: "Test Sample 1",
        },
      ],
    };
  },
  methods: {
    // deleteItem() {
    //   this.DialogDeny = true;
    // },
    handleFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        // Reset imageUrl before updating with the new image
        this.imageUrl = null;

        this.imageUrl = URL.createObjectURL(file);
        this.$emit("image-selected", this.imageUrl);
        // You can emit this imageUrl to the parent component if needed
      }
    },

    Rowclick() {
      this.editedItem = {
        id: null,
        announcementname: "",
      };
      this.CreationDialog = true;
    },

    cancel() {
      this.CreationDialog = false;
    },

    save() {
      this.CreationDialog = false;
    },

    editItem(item) {
      this.CreationDialog = true;
    },
  },
};
</script>
<style scoped>
.placeholder {
  border: 2px dashed #ccc;
  padding-top: 80px;
  text-align: center;
  color: #777;
  width: 200px;
  height: 195px;
}
.imunizationbackgroundcolor {
  background-color: black;
  color: white;
}
.imunizationbackgroundcolor_caption {
  background-color: #057407;
  color: white;
  margin-top: -13%;
}

.rowup {
  margin-top: -4%;
}

.rowup1 {
  margin-top: -3%;
}
.dashboard {
  padding: 2%;
  justify-content: space-between;
  display: flex;
  flex-wrap: wrap;
}

.my-card {
  box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.2);
  margin: 1%;
  display: flex;
  flex-direction: column;
  height: 100%;
}
</style>
