<style lang="less">
@import "./snack.less";
@import "../../styles/common.less";
</style>
<template>
      <Row>
        <Col span="12">
            <Rate show-text v-model="valueText"></Rate>
        </Col>
        <Col span="12">
            <Rate show-text allow-half v-model="valueCustomText">
                <span style="color: #f5a623">{{ valueCustomText }}</span>
            </Rate>
        </Col>
    </Row>
</template>

<script>
import util from "@/libs/util.js";
export default {
  name: "snacks",
  data() {
    return {
      valueText: 3,
      valueCustomText: 3.8
    };
  }
};
</script>
