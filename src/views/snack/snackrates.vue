<style lang="less">
@import "./snack.less";
@import "../../styles/common.less";
</style>
<template>
    <Card>
    <p slot="title">
            <Icon type="ios-plus-outline"></Icon>
            评价小吃
            {{this.$route.params}}
        </p>

<Scroll :on-reach-bottom="handleReachBottom">
        <Card dis-hover v-for="(rate, index) in rates" :key="index" style="margin: 32px 0">
        <FormItem label="评价">
           <Rate disabled allow-half v-model="rate.value">
            </Rate>
        </FormItem>

        <FormItem label="描述">
          <label>{{rate.description}}</label>
            <Input v-model="rate.description" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入..."></Input>
        </FormItem>

        </Card>
    </Scroll>

    </Card>
</template>

<script>
import util from "@/libs/util.js";
export default {
  name: "snacks",
  data() {
    return {
      rates: []
    };
  },
  methods: {
    getRates: function(snackId) {
      util.ajax
        .get(`snacks/${snackId}/rates`)
        .then(response => {
          this.rates = response.data;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    createRate: function() {
      var snackId = this.$route.params.snack_id;
      util.ajax
        .post(`snacks/${snackId}/rates`, this.rate)
        .then(response => {
          this.$Message.success("添加成功!");
        })
        .catch(e => {});
    }
  },
  mounted() {
    this.getSnacks();
  }
};
</script>