<script>
export default {
  name: "Internal-Input",
  props: {
    // 数据模型
    model: {
      type: Object,
      default() {
        return {};
      }
    },
    rules: {
      type: Object,
      requried: true
    },
    config: {
      type: Array,
      requried: false
    }
  },

  data() {
    return {
      updating: false,
      formValues: this.mergeValues()
    };
  },

  watch: {
    formValues: {
      handler(formValues) {
        this.updating = true;
        this.$emit("input", formValues);
      },
      deep: true
    },
    model: {
      handler() {
        if (!this.updating) {
          this.formValues = this.mergeValues();
        } else {
          this.updating = false;
        }
      },
      deep: true
    }
  },
  render(h) {
    let vm = this;
    return h(
      "Form",
      {
        props: {
          model: vm.formValues,
          rules: vm.rules,
          labelWidth: 80
        }
      },
      [
        ...(vm.$slots.prepend || []),
        ...(vm.renderFormItems(h) || []),
        ...(vm.$slots.append || [])
      ]
    );
  },
  created() {
    this.$emit("input", this.formValues);
  },
  methods: {
    mergeValues() {
      const vm = this;
      const { model } = vm;
      const defaultValues = {};
      this.config.forEach(({ tag, detail = {} }) => {
        console.log(tag);
        let { defaultValue = null, name } = detail;
        defaultValues[name] = defaultValue;
      });

      return {
        ...defaultValues,
        ...model
      };
    },
    filterAttrs(detail = {}) {
      const keys = Object.keys(detail);
      const attrs = {};
      keys.forEach(key => {
        const value = detail[key];
        if (
          typeof value === "number" ||
          typeof value === "string" ||
          typeof value === "boolean"
        ) {
          attrs[key] = value;
        }
      });
      return attrs;
    },
    renderFormItem(h, component, label, formItemName) {
      const vm = this;
      const tag = component.$type;
      const { formValues } = vm;
      const { props } = component;
      const value = formValues[formItemName] || null;
      const modelEvents = {
        input: function(value) {
          formValues[name] = value;
        }
      };
      let children = [];

      if (tag === "Select") {
        const select = h(
          tag,
          {
            attrs: {
              ...vm.filterAttrs(props)
            },
            props: {
              value,
              ...props
            },
            on: {
              ...modelEvents
            }
          },
          (component.items || []).map(option => {
            return h("Option", {
              attrs: {
                ...vm.filterAttrs(option)
              },
              props: {
                key: option.value,
                ...option
              }
            });
          })
        );
        children = [select];
      } else if (tag === "CheckboxGroup") {
        const checkbox = h(
          "CheckboxGroup",
          {
            attrs: {
              ...vm.filterAttrs(props)
            },
            props: {
              value,
              ...props
            },
            on: {
              ...modelEvents
            }
          },
          (component.items || []).map(option => {
            return h("Checkbox", {
              attrs: {
                ...vm.filterAttrs(option)
              },
              props: {
                key: option.value,
                label: option.label
              }
            });
          })
        );
        children = [checkbox];
      } else if (tag === "RadioGroup") {
        const radioGroup = h(
          "RadioGroup",
          {
            attrs: {
              ...vm.filterAttrs(props)
            },
            props: {
              value,
              ...props
            },
            on: {
              ...modelEvents
            }
          },
          (component.items || []).map(option => {
            return h("Radio", {
              attrs: {
                ...vm.filterAttrs(option)
              },
              props: {
                key: option.label,
                label: option.label
              }
            });
          })
        );
        children = [radioGroup];
      } else {
        const input = h(tag || "Input", {
          attrs: {
            ...vm.filterAttrs(props)
          },
          props: {
            value,
            ...props
          },
          on: {
            ...modelEvents
          }
        });
        children = [input];
      }

      return h(
        "FormItem",
        {
          props: {
            prop: formItemName,
            label: label
          }
        },
        children
      );
    },

    renderFormItems(h) {
      const vm = this;
      return this.config.map(item => {
        return vm.renderFormItem(h, item.component, item.label, item.key);
      });
    }
  }
};
</script>
