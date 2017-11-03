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
                    <Row class="margin-top-10">
                        <Table :columns="snackColumns" :data="snacks"></Table>
                    </Row>
                </Card>
            </Col>
        </Row>

    </div>
</template>

<script>
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
          key: "province",
          title: "省"
        },
                {
          key: "city",
          title: "市"
        },
        {
          key: "taste",
          title: "味道"
        },
        {
          key: "texture",
          title: "口感"
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
        content: `名字：${this.snacks[index].name}<br>省：${this.snacks[index].province || ""}<br>市：${this.snacks[index].city || ""}<br>价钱：${this.snacks[index]
          .price}<br>味道：${this.snacks[index].taste || ""}<br>口感：${this.snacks[index].texture || ""}<br>描述：${this.snacks[index].description || ""}`
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
              province: response.data[i].province,
              city: response.data[i].city,
              taste: response.data[i].taste.toString(),
              texture: response.data[i].texture.toString(),
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
