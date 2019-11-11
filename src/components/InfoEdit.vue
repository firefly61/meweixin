<template>
  <div class="infoEdit">
    <h1 class="title-box">信息完善</h1>
    <cube-form :model="model" @validate="validateHandler" @submit="submitHandler">
      <cube-form-group>
        <cube-form-item :field="fields[0]"></cube-form-item>
        <cube-form-item :field="fields[1]"></cube-form-item>
        <cube-form-item :field="fields[2]"></cube-form-item>
        <cube-form-item :field="fields[3]"></cube-form-item>
        <cube-form-item :field="fields[4]">
          <cube-button class="date-btn" @click="showDatePicker">{{model.dateValue || 'Please select date'}}</cube-button>
          <date-picker ref="datePicker" title="请选择日期" :min="new Date(1960, 1, 1)" :max="new Date()" :value="new Date()"
            @select="dateSelectHandler">
          </date-picker>
        </cube-form-item>
      </cube-form-group>
      <cube-form-group>
        <cube-button type="submit">Submit</cube-button>
      </cube-form-group>
    </cube-form>
  </div>
</template>

<script>
  import {
    DatePicker
  } from 'cube-ui';
  export default {
    name: "InfoEdit",
    data() {
      return {
        model: {
          name: '刘颖',
          phone: '13611111123',
          sexy: 2,
          dateValue: this.dateFormat(new Date())
        },
        fields: [{
            type: 'input',
            modelKey: 'name',
            label: '真实姓名',
            props: {
              placeholder: '请输入姓名'
            },
            rules: {
              required: true
            }
          },
          {
            type: 'input',
            modelKey: 'phone',
            label: '电话',
            props: {
              placeholder: '请输入电话号'
            },
            rules: {
              required: true
            }
          },
          {
            type: 'input',
            modelKey: 'card',
            label: '身份证',
            props: {
              placeholder: '请输入证件号'
            },
            rules: {
              required: true
            },
            events: {
              blur(e) {
                console.log(e.target.value)
              }
            }
          },
          {
            type: 'radio-group',
            modelKey: 'sexy',
            label: '性别',
            rules: {
              required: true
            },
            props: {
              options: [{
                label: '男',
                value: 1
              }, {
                label: '女',
                value: 2
              }]
            }
          },
          {
            modelKey: 'dateValue',
            label: '生日',
            rules: {
              required: true
            }
          }
        ]
      }
    },
    methods: {
      submitHandler() {
        console.log(this.model)
      },
      validateHandler(result) {
        this.validity = result.validity
        this.valid = result.valid
        console.log('validity', result.validity, result.valid, result.dirty, result.firstInvalidFieldIndex)
      },
      showDatePicker() {
        this.$refs.datePicker.show()
      },
      dateSelectHandler(date, selectedVal, selectedText) {
        console.log(selectedVal, selectedText);
        this.model.dateValue = this.dateFormat(date);
      },
      dateFormat(date) {
        let year = date.getFullYear(),
        month = date.getMonth() + 1,
        day = date.getDate();
        return year + '-' + month + '-' + day;
      }
    },
    components: {
      DatePicker
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title-box {
  padding: 1rem;
  font-size: 1.25rem;
}
.date-btn {
  background: transparent;
  text-align: left;
  color: inherit;
  padding-left: 0;
}
</style>