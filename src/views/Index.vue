<template>
  <div class="wrapper">
    <Row>
      <i-col class="border left" span="8">
        <form-item-engine :config="config" :rules="rules" :model="formValues" />
      </i-col>
      <i-col class="border left" span="8"> </i-col>
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
  data() {
    return {
      formValues: {
        name: "11",
        city: 1,
        fruit: []
      },
      rules: {
        name: [
          {
            required: true,
            message: "The name cannot be empty",
            trigger: "blur"
          }
        ],
        city: [
          {
            required: true,
            message: "Please select the city",
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
