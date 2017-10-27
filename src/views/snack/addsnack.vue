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
 <Form :model="formItem" :label-width="80">
        <FormItem label="小吃名字">
            <Input v-model="formItem.input" placeholder="请输入"></Input>
        </FormItem>
        <FormItem label="省">
            <Select v-model="formItem.select" placeholder="请选择">
                <Option value="beijing">北京市</Option>
                <Option value="shanghai">上海市</Option>
                <Option value="shenzhen">深圳市</Option>
            </Select>
        </FormItem>
            <FormItem label="市">
            <Select v-model="formItem.select" placeholder="请选择">
                <Option value="beijing">北京市</Option>
                <Option value="shanghai">上海市</Option>
                <Option value="shenzhen">深圳市</Option>
            </Select>
        </FormItem>

        <FormItem label="价格">
            <RadioGroup v-model="formItem.radio">
                <Radio label="1">$</Radio>
                <Radio label="2">$$</Radio>
                <Radio label="3">$$$</Radio>
                <Radio label="4">$$$$</Radio>
            </RadioGroup>
        </FormItem>
        <FormItem label="味道">
            <CheckboxGroup v-model="formItem.checkbox">
                <Checkbox label="酸"></Checkbox>
                <Checkbox label="甜"></Checkbox>
                <Checkbox label="辣"></Checkbox>
                <Checkbox label="苦"></Checkbox>
                <Checkbox label="特殊味道"></Checkbox>
            </CheckboxGroup>
        </FormItem>
        <FormItem label="描述">
            <Input v-model="formItem.textarea" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入..."></Input>
        </FormItem>



        <FormItem>
            <Button type="primary">提交</Button>
            <Button type="ghost" style="margin-left: 8px">取消</Button>
        </FormItem>
    </Form>
    </Card>

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
  data() {
    return {
      formItem: {
        input: "",
        select: "",
        radio: "male",
        checkbox: [],
        switch: true,
        date: "",
        time: "",
        slider: [20, 50],
        textarea: ""
      }
    };
  },
  methods: {
    createSnack() {
      util.ajax
        .post(`snacks`, this.formItem)
        .then(response => {
          //this.snacks = response.data;
        //   for (var i = 0; i < response.data.length; i++) {
        //     this.snacks.push({
        //       name: response.data[i].name,
        //       image: response.data[i].image_url
        //     });
        //   }
        })
        .catch(e => {
          this.errors.push(e);
        });
    }
  }
};
</script>
