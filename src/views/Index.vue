<template>
  <div class="wrapper">
    <Row>
      <i-col class="border left" span="8">
        <form-item-engine :config="config" :rules="rules" :model="formValues">
          <div slot="append" class="submit-item">
            <Button type="primary" @on-click="showResultModal = true">
              提交
            </Button>
          </div>
        </form-item-engine>
      </i-col>
      <i-col class="border left" span="8">
        <Input type="textarea" :rows="35" v-model="configShow" />
      </i-col>
      <i-col class="border" span="8"> </i-col>
    </Row>
  </div>
</template>
<script>
import FormItemEngine from "./form-engine";
export default {
  components: {
    FormItemEngine
  },
  watch: {
    config: {
      deep: true,
      immediate: true,
      handler(value) {
        debugger;
        console.log(value);
      }
    }
  },
  computed: {
    configShow: function() {
      debugger;
      return JSON.stringify(this.config, null, 4);
    }
  },
  data() {
    return {
      formValues: {
        name: "",
        city: 1,
        fruit: [],
        article: "",
        animal: ""
      },
      rules: {
        name: [
          {
            required: true,
            message: "姓名不能为空",
            trigger: "blur"
          }
        ],
        city: [
          {
            required: true,
            message: "城市不能为空",
            trigger: "change"
          }
        ]
      },
      config: [
        {
          key: "name",
          label: "姓名",
          prop: "name",
          component: {
            $type: "Input",
            value: 1,
            attrs: {},
            on: {},
            props: {
              placeholder: "请输入姓名"
            }
          }
        },
        {
          key: "city",
          label: "城市",
          prop: "city",
          component: {
            $type: "Select",
            props: {
              placeholder: "请选择城市",
              clearable: true,
              filterable: true
            },
            items: [
              {
                value: 1,
                label: "测试1"
              },
              {
                value: 2,
                label: "测试2"
              },
              {
                value: 3,
                label: "测试3"
              },
              {
                value: 4,
                label: "测试4"
              },
              {
                value: 5,
                label: "测试5"
              }
            ]
          }
        },
        {
          key: "fruit",
          label: "种类",
          prop: "fruit",
          component: {
            $type: "CheckboxGroup",
            props: {},
            items: [
              {
                label: "🍎"
              },
              {
                label: "🍌"
              },
              {
                label: "🍑"
              },
              {
                label: "🍒"
              },
              {
                label: "🍎"
              },
              {
                label: "🍉"
              },
              {
                label: "🍊"
              }
            ]
          }
        },
        {
          key: "article",
          label: "文章",
          prop: "article",
          component: {
            $type: "Input",
            value: 1,
            attrs: {},
            on: {},
            props: {
              type: "textarea",
              placeholder: "请填写文章"
            }
          }
        },
        {
          key: "animal",
          label: "动物",
          prop: "animal",
          component: {
            $type: "RadioGroup",
            props: {},
            items: [
              {
                label: "老虎🐯"
              },
              {
                label: "大象🐘"
              },
              {
                label: "蛇🐍"
              }
            ]
          }
        }
      ]
    };
  },
  methods: {}
};
</script>

<style scoped>
.wrapper {
  margin: 40px;
  margin-top: 80px;
}
.border {
  border: 1px solid #dcdee2;
  height: 80vh;
  padding: 20px;
}
.left {
  border-right: none;
}
</style>
