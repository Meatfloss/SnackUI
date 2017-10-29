<style lang="less">
@import "./snack.less";
@import "../../styles/common.less";
</style>
<template>
<Card>
    <p slot="title">
            <Icon type="ios-plus-outline"></Icon>
            请输入小吃信息
        </p>
 <Form :model="snack" :label-width="80">
        <FormItem label="小吃名字">
            <Input v-model="snack.name" placeholder="请输入"></Input>
        </FormItem>
        <FormItem label="省">
            <Select v-model="snack.select" placeholder="请选择">
                <Option v-for="p in provinces">{{p}}</Option>
            </Select>
        </FormItem>
            <FormItem label="市">
            <Select v-model="snack.select" placeholder="请选择">
                <Option value="beijing">北京市</Option>
                <Option value="shanghai">上海市</Option>
                <Option value="shenzhen">深圳市</Option>
            </Select>
        </FormItem>

        <FormItem label="价格">
            <RadioGroup v-model="snack.price">
                <Radio label="$"></Radio>
                <Radio label="$$"></Radio>
                <Radio label="$$$"></Radio>
                <Radio label="$$$$"></Radio>
            </RadioGroup>
        </FormItem>
        <FormItem label="味道">
            <CheckboxGroup v-model="snack.taste">
                <Checkbox label="酸"></Checkbox>
                <Checkbox label="甜"></Checkbox>
                <Checkbox label="香"></Checkbox>
                <Checkbox label="辣"></Checkbox>
                <Checkbox label="特殊味道"></Checkbox>
            </CheckboxGroup>
        </FormItem>
            <FormItem label="口感">
            <CheckboxGroup v-model="snack.texture">
                <Checkbox label="软"></Checkbox>
                <Checkbox label="滑"></Checkbox>
                <Checkbox label="酥"></Checkbox>
                <Checkbox label="脆"></Checkbox>
                <Checkbox label="特别"></Checkbox>
            </CheckboxGroup>
        </FormItem>
        <FormItem label="描述">
            <Input v-model="snack.description" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入..."></Input>
        </FormItem>



        <FormItem>
            <Button type="primary" @click="createSnack">提交</Button>
            <Button type="ghost" style="margin-left: 8px">取消</Button>
        </FormItem>
    </Form>
    </Card>

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
import {provinces} from "./data/city-data.js"
import util from "@/libs/util.js";

export default {
  data() {
    return {
      snack: this.getNewSnack(),
      provinces: provinces
      //provinces: ['北京','上海']
    };
  },
  methods: {
    createSnack() {
      util.ajax
        .post(`snacks`, this.snack)
        .then(response => {
          this.$Message.success("添加成功");

          //clear form
          this.snack = this.getNewSnack();
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    getNewSnack() {
      return {
        name: "",
        select: "",
        price: "$",
        taste: [],
        description: ""
      };
    }
  }
};
</script>
