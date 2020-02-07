<template>
  <div class="ui menu">
    <a class="active item">
      SMCT
    </a>
    <div class="right menu">
      <button
        type="button"
        class="ui secondaty button"
        data-toggle="modal"
        data-target="#exampleModalLong"
      >
        Create a new Campaign
      </button>
      <div
        class="modal fade"
        id="exampleModalLong"
        tabindex="-1"
        role="dialog"
        aria-labelledby="exampleModalLongTitle"
        aria-hidden="true"
      >
        <form action="https://smct-bbb.herokuapp.com/campaigns" method="POST">
          <div class="modal-dialog" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLongTitle">
                  New Campaign
                </h5>
                <button
                  id="close"
                  ref="closeButton"
                  type="button"
                  class="close"
                  data-dismiss="modal"
                  aria-label="Close"
                >
                  <span aria-hidden="true">&times;</span>
                </button>
              </div>
              <div class="modal-body">
                <div class="form-group row">
                  <label for="title" class="col-sm-2 col-form-label"
                    >Campaign title</label
                  >
                  <div class="col-sm-10">
                    <input
                      name="title"
                      required
                      ref="title"
                      type="text"
                      class="form-control"
                      id="title"
                      placeholder="Title"
                    />
                  </div>
                </div>
                <div class="form-group row">
                  <div class="form-group">
                    <label for="exampleFormControlFile1">Campaign image</label>
                    <input
                      type="file"
                      id="file"
                      ref="file"
                      v-on:change="onChangeFileUpload()"
                    />
                  </div>
                </div>

                <p>Image Preview</p>
                <img
                  class="canvas"
                  ref="preview"
                  src=""
                  alt=""
                  width="inherit"
                  height="inherit"
                />
              </div>
              <div class="modal-footer">
                <button type="reset" class="btn btn-secondary">Reset</button>
                <button
                  ref="submitButton"
                  :class="{ hide: loading }"
                  type="submit"
                  class="btn btn-primary"
                  @click.prevent="submitCampaign"
                >
                  Submit
                </button>
                <button
                  disabled
                  class="ui primary loading button"
                  :class="{ hide: !loading }"
                >
                  Loading
                </button>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import Axios from "axios";
import List from "./List";
export default {
  name: "HelloWorld",
  props: {},
  data() {
    return {
      file: "",
      loading: false
    };
  },
  methods: {
    submitCampaign: async function() {
      // console.log(this.$refs);
      if (!this.$refs.title.value) alert("title cannot be empty");
      else var formData = new FormData();
      formData.append("file", this.file);
      formData.append("title", this.$refs.title.value);
      formData.append("session", sessionStorage.getItem("token"));
      this.loading = true;

      const resp = await Axios.post(
        "https://smct-bbb.herokuapp.com/campaigns",
        formData,
        {
          headers: {
            "Content-Type": "multipart/form-data"
          }
        }
      );
      this.loading = false;
      this.$refs.closeButton.click();
      location.reload();
    },
    onChangeFileUpload() {
      this.$refs.preview.src = URL.createObjectURL(this.$refs.file.files[0]);
      this.file = this.$refs.file.files[0];
    }
  },
};
</script>

<style scoped>
.hide {
  visibility: hidden;
  display: none;
}
#canvas {
  width: 300px;
  height: 40px;
  padding: 200px 20px;
  text-align: center;
  border: 1px solid #333;
}
.canvas {
  width: 300px;
  height: 300px;
}
</style>
