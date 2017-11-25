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
    <Scroll :on-reach-bottom="handleReachBottom" :height="550">
        <Card dis-hover v-for="(rate, index) in rates" :key="index" style="margin: 32px 10px">
            <Rate disabled allow-half v-model="rate.value">
            </Rate>
          <label>{{rate.description}}</label>
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
      currentIndex: 1,
      showEnd: false
    };
  },
  methods: {
    getRates: function(index) {
      var snackId = this.$route.params.snack_id;
      util.ajax
        .get(`snacks/${snackId}/rates/${index}`)
        .then(response => {
          if(!response.data || !response.data.length) this.showEnd = true;
          this.rates.push.apply(this.rates, response.data);
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
    handleReachBottom() {
      if(this.showEnd) return;
      return new Promise(resolve => {
        setTimeout(() => {
          // const last = this.rates[this.rates.length - 1];
          // for (let i = 1; i < 11; i++) {
          //   this.rates.push(last + i);
          // }
          this.getRates(this.currentIndex++);
          resolve();
        }, 2000);
      });
    }
  },
  mounted() {
    this.getRates(0);
  }
};
</script>