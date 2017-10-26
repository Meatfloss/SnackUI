<style lang="less">
@import "./snack.less";
@import "../../styles/common.less";
</style>
<template>
    <div class="home-main">

        <Row class="margin-top-10">
            <Col :xs="24" :lg="12">
                <Card>
                    <p slot="title">
                        <Icon type="pinpoint"></Icon>
                        小吃列表
                    </p>
                    <!-- <Row>
                        <Input v-model="searchConName1" icon="search" @on-change="handleSearch1" placeholder="请输入姓名搜索..." style="width: 200px" />
                    </Row> -->
                    <Row class="margin-top-10 searchable-table-con1">
                        <Table :columns="snackColumns" :data="snacks"></Table>
                    </Row>
                </Card>
            </Col>
        </Row>
        <Row class="margin-top-10">
            <Col span="8">
                <Card>
                    <p slot="title" class="card-title">
                        <Icon type="android-map"></Icon>
                        上周每日来访量统计
                    </p>
                    <div class="data-source-row">
                        <visite-volume></visite-volume>
                    </div>
                </Card>
            </Col>
            <Col span="8" class="padding-left-10">
                <Card>
                    <p slot="title" class="card-title">
                        <Icon type="ios-pulse-strong"></Icon>
                        数据来源统计
                    </p>
                    <div class="data-source-row">
                        <data-source-pie></data-source-pie>
                    </div>
                </Card>
            </Col>
            <Col span="8" class="padding-left-10">
                <Card>
                    <p slot="title" class="card-title">
                        <Icon type="android-wifi"></Icon>
                        各类用户服务调用变化统计
                    </p>
                    <div class="data-source-row">
                        <user-flow></user-flow>
                    </div>
                </Card>
            </Col>
        </Row>
        <Row class="margin-top-10">
            <Card>
                <p slot="title" class="card-title">
                    <Icon type="ios-shuffle-strong"></Icon>
                    上周每日服务调用量(万)
                </p>
                <div class="line-chart-con">
                    <service-requests></service-requests>
                </div>
            </Card>
        </Row>
    </div>
</template>

<script>
import cityData from "./map-data/get-city-value.js";
import homeMap from "./components/map.vue";
import dataSourcePie from "./components/dataSourcePie.vue";
import visiteVolume from "./components/visiteVolume.vue";
import serviceRequests from "./components/serviceRequests.vue";
import userFlow from "./components/userFlow.vue";
import countUp from "./components/countUp.vue";
import inforCard from "./components/inforCard.vue";
import mapDataTable from "./components/mapDataTable.vue";
import toDoListItem from "./components/toDoListItem.vue";
import util from "@/libs/util.js";

export default {
  name: "home",
  components: {
    homeMap,
    dataSourcePie,
    visiteVolume,
    serviceRequests,
    userFlow,
    countUp,
    inforCard,
    mapDataTable,
    toDoListItem
  },
  data() {
    return {
      toDoList: [
        {
          title: "去iView官网学习完整的iView组件"
        },
        {
          title: "去iView官网学习完整的iView组件"
        },
        {
          title: "去iView官网学习完整的iView组件"
        },
        {
          title: "去iView官网学习完整的iView组件"
        },
        {
          title: "去iView官网学习完整的iView组件"
        }
      ],
      count: {
        createUser: 496,
        visit: 3264,
        collection: 24389305,
        transfer: 39503498
      },
      cityData: cityData,
      showAddNewTodo: false,
      newToDoItemValue: "",
      snacks: [],
      snackColumns: [
        {
          key: "name",
          title: "名字"
        },
        {
          key: "image",
          title: "图片链接"
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
    getSnacks() {
      util.ajax
        .get(`snacks`)
        .then(response => {
          //this.snacks = response.data;
          for (var i = 0; i < response.data.length; i++) {
            this.snacks.push({
              name: response.data[i].name,
              image: response.data[i].image_url
            });
          }
        })
        .catch(e => {
          this.errors.push(e);
        });
    },

    addNewToDoItem() {
      this.showAddNewTodo = true;
    },
    addNew() {
      if (this.newToDoItemValue.length !== 0) {
        this.toDoList.unshift({
          title: this.newToDoItemValue
        });
        setTimeout(function() {
          this.newToDoItemValue = "";
        }, 200);
        this.showAddNewTodo = false;
      } else {
        this.$Message.error("请输入待办事项内容");
      }
    },
    cancelAdd() {
      this.showAddNewTodo = false;
      this.newToDoItemValue = "";
    }
  },
  mounted() {
    this.getSnacks();
  }
};
</script>
