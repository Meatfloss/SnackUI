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
    <p @click="toggleAddRage"><Icon type="plus" />添加评论</p>
        <Card dis-hover style="margin: 20px 10px" v-if="showAddRate">
     <Form :model="rate" :label-width="80">
        <FormItem label="评价">
           <Rate show-text allow-half v-model="rate.value">
                <span style="color: #f5a623">{{ rate.value }}</span>
            </Rate>
        </FormItem>
        <FormItem label="主题">
            <Input v-model="rate.subject" placeholder="主题"></Input>
        </FormItem>
        <FormItem label="描述">
            <Input v-model="rate.description" type="textarea" :autosize="{minRows: 2,maxRows: 5}" placeholder="请输入..."></Input>
        </FormItem>
        <FormItem>
            <Button type="primary" @click="createRate">提交</Button>
            <Button type="ghost" @click="clearInputs" style="margin-left: 8px">清空</Button>
        </FormItem>
    </Form>
    </Card>
    <Scroll :on-reach-bottom="handleReachBottom" :height="550">
        <Card dis-hover v-for="(rate, index) in rates" :key="index" style="margin: 20px 10px">
          <Row>
            <Rate disabled allow-half v-model="rate.value">
            </Rate>
          <label>{{rate.subject}}</label>
          </Row>
          <Row>By {{rate.user_name}} {{rate.createdAt}}</Row>
          <Row><label>{{rate.description}}</label></Row>
        </Card>
        <Card v-if="showEnd">
          <label>没有评论了</label>
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
      rates: [],
      rate: {
        value: 0,
        subject: "",
        description: "",
        user_name: "tester",
        snack_id: this.$route.params.snack_id
      },
      currentIndex: 1,
      showEnd: false,
      showAddRate: false
    };
  },
  methods: {
    getRates: function(index) {
      var snackId = this.$route.params.snack_id;
      util.ajax
        .get(`snacks/${snackId}/rates/${index}`)
        .then(response => {
          if (!response.data || !response.data.length) this.showEnd = true;
          else {
            for (var i = 0; i < response.data.length; i++) {
              response.data[i].createdAt = response.data[i].createdAt.slice(
                0,
                response.data[i].createdAt.indexOf("T")
              );
            }
            this.rates.push.apply(this.rates, response.data);
          }
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
    },
    handleReachBottom: function() {
      if (this.showEnd) return;
      return new Promise(resolve => {
        setTimeout(() => {
          this.getRates(this.currentIndex++);
          resolve();
        }, 2000);
      });
    },
    toggleAddRage: function(){
      this.showAddRate = !this.showAddRate;
    },
    clearInputs: function(){
      this.rate.value = 0;
      this.rate.subject = "";
      this.rate.description = "";
    }
  },
  mounted() {
    this.getRates(0);
  }
};
</script>