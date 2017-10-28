<style lang="less">
@import "./snack.less";
@import "../../styles/common.less";
</style>
<template>
    <div class="home-main">

        <Row class="margin-top-10">
            <Col :xs="24" :lg="24">
                <Card>
                    <p slot="title">
                        <Icon type="pinpoint"></Icon>
                        小吃列表
                    </p>
                    <!-- <Row>
                        <Input v-model="searchConName1" icon="search" @on-change="handleSearch1" placeholder="请输入姓名搜索..." style="width: 200px" />
                    </Row> -->
                    <Row class="margin-top-10">
                        <Table :columns="snackColumns" :data="snacks"></Table>
                    </Row>
                </Card>
            </Col>
        </Row>

    </div>
</template>

<script>
// import cityData from "./map-data/get-city-value.js";
// import homeMap from "./components/map.vue";
// import dataSourcePie from "./components/dataSourcePie.vue";
// import visiteVolume from "./components/visiteVolume.vue";
// import serviceRequests from "./components/serviceRequests.vue";
// import userFlow from "./components/userFlow.vue";
// import countUp from "./components/countUp.vue";
// import inforCard from "./components/inforCard.vue";
// import mapDataTable from "./components/mapDataTable.vue";
// import toDoListItem from "./components/toDoListItem.vue";
import util from "@/libs/util.js";

export default {
  name: "snacks",
  data() {
    return {
      snacks: [],
      snackColumns: [
        {
          key: "name",
          title: "名字"
        },
        {
          key: "price",
          title: "价格",
          sortable: true
        },
        {
          key: "taste",
          title: "味道"
        },
        {
          key: "description",
          title: "描述"
        },
        {
          key: "image",
          title: "图片链接"
        }
        ,
        {
          title: "操作",
          key: "action",
          width: 150,
          align: "center",
          render: (h, params) => {
            return h("div", [
              h(
                "Button",
                {
                  props: {
                    type: "primary",
                    size: "small"
                  },
                  style: {
                    marginRight: "5px"
                  },
                  on: {
                    click: () => {
                      this.show(params.index);
                    }
                  }
                },
                "查看"
              ),
              h(
                "Button",
                {
                  props: {
                    type: "error",
                    size: "small"
                  },
                  on: {
                    click: () => {
                      this.remove(params.index);
                    }
                  }
                },
                "删除"
              )
            ]);
          }
        }
      ]
    };
  },
  computed: {
    avatorPath() {
      return localStorage.avatorImgPath;
    }
  },
  methods: {
    show(index) {
      this.$Modal.info({
        title: "用户信息",
        content: `名字：${this.snacks[index].name}<br>价钱：${this.snacks[index]
          .price}<br>味道：${this.snacks[index].taste || ""}<br>描述：${this.snacks[index].description || ""}`
      });
    },
    remove(index) {
      this.deleteSnack(this.snacks[index]);
      this.snacks.splice(index, 1);

    },
    getSnacks() {
      util.ajax
        .get(`snacks`)
        .then(response => {
          //this.snacks = response.data;
          for (var i = 0; i < response.data.length; i++) {
            this.snacks.push({
              _id: response.data[i]._id,
              name: response.data[i].name,
              price: response.data[i].price,
              taste: response.data[i].taste.toString(),
              description: response.data[i].description,
              image: response.data[i].image_url
            });
          }
        })
        .catch(e => {
          this.errors.push(e);
        });
    },

    deleteSnack(snack) {
      util.ajax
        .delete(`snacks/${snack._id}`)
        .then(response => {
          //this.snacks = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
  },
  mounted() {
    this.getSnacks();
  }
};
</script>
