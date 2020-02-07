<template>
  <div class="List">
    <div
      v-for="(item, index) in list"
      v-bind:key="index"
      class="ui four column grid"
    >
      <div class="column li-i">
        {{ item.title }}
      </div>
      <div class="column li-i">
        {{ item.createdAt }}
      </div>
      <div class="column li-i">
        {{ item.publishedAAt ? item.publishedAAt : "Not Pulbished yet " }}
      </div>
      <div class="column li-buttons">
        <button
          type="button"
          class="ui inverted violet button"
          data-toggle="modal"
          @click="preview(index)"
        >
          Preview
        </button>

        <!-- Modal -->
        <div
          class="modal fade"
          :id="index"
          tabindex="-1"
          role="dialog"
          aria-labelledby="exampleModalCenterTitle"
          aria-hidden="true"
        >
          <div class="modal-dialog modal-dialog-centered" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLongTitle">
                  Campaign Preview
                </h5>
                <button
                  type="button"
                  class="close"
                  data-dismiss="modal"
                  aria-label="Close"
                >
                  <span aria-hidden="true">&times;</span>
                </button>
              </div>
              <div class="modal-body">
                <h3>{{ item.title }}</h3>
                <img :src="item.image" alt="" srcset="" />
              </div>
              <div class="modal-footer">
                <button
                  type="button"
                  class="btn btn-secondary"
                  data-dismiss="modal"
                >
                  Close
                </button>
              </div>
            </div>
          </div>
        </div>

        <button class="ui yellow button">Publish</button>
        <button class="ui red button" @click="del(index)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
// import ListItem from './ListItem'
import axios from "axios";
import Axios from "axios";

export default {
  name: "List",
  components: {},
  // props: {
  //   list : Array
  // }
  data() {
    return {
      list: [
        {
          title: "hi",
          createdAt: "21-22-20",
          publishedOn: "2-22-21"
        }
      ]
    };
  },
  methods: {
    startSession: function() {
      axios.get("https://smct-bbb.herokuapp.com/session").then(res => {
        sessionStorage.setItem("token", res.data.id);
      });
    },
    getCampaigns: function() {
      Axios.get(
        `https://smct-bbb.herokuapp.com/campaigns/${sessionStorage.token}`
      )
        .then(res => {
          for (var i = 0; i < res.data.length; i++) res.data[i].dt = `#${i}`;
          this.list = res.data;
        })
        .catch(err =>alert(err));
    },
    preview: function(index) {
      const campaign = this.list[index];
      this.$swal({
        html: `<h1>${campaign.title} </h1>
        <img height="300px" width="300px" src="${campaign.image}"/>
        `
      });
    },
    del: async function(index) {
      const campaign = this.list[index];
      if (confirm("Are you sure you want to delete this campaign?")) {
        await Axios.delete(
          `https://smct-bbb.herokuapp.com/campaigns/${campaign._id}`
        );
        this.$swal("deleted");
        location.reload();
      }
    }
  },
  beforeMount() {
    if (!sessionStorage.getItem("token")) this.startSession();
    else this.getCampaigns();
  }
};
</script>

<style scoped>
.li-i {
  margin-top: 10px;
}
</style>
