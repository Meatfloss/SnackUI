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
 <Form :model="rate" :label-width="80">
        <!-- <FormItem label="小吃名字">
            <Input v-model="rate.snack.name" placeholder="请输入"></Input>
        </FormItem> -->
        <FormItem label="评价">
           <Rate show-text allow-half v-model="rate.value">
                <span style="color: #f5a623">{{ rate.value }}</span>
            </Rate>
        </FormItem>

        <FormItem label="描述">
            <Input v-model="rate.description" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入..."></Input>
        </FormItem>


        <FormItem>
            <Button type="primary" @click="createRate">提交</Button>
            <Button type="ghost" style="margin-left: 8px">取消</Button>
        </FormItem>
    </Form>
    </Card>
</template>

<script>
import util from "@/libs/util.js";
export default {
  name: "snacks",
  data() {
    return {
      rate: {
        value: 1,
        description: "TESt"
      }
    };
  },
  methods: {
    getRates: function(snackId) {
      util.ajax
        .get(`snacks/${snackId}/rates`)
        .then(response => {

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
        .catch(e => {

        });
    }
  }
};
</script>
