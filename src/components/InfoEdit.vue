<template>
  <div id="infoEdit">
    <h1 class="title-box">信息完善</h1>
    <cube-form :model="model" @validate="validateHandler" @submit="submitHandler">
      <cube-form-group>
        <cube-form-item :field="fields[0]">
        </cube-form-item>
        <cube-form-item :field="fields[1]">
        </cube-form-item>
        <cube-form-item :field="fields[2]"></cube-form-item>
        <cube-form-item :field="fields[3]"></cube-form-item>
        <cube-form-item :field="fields[4]">
          <cube-button class="date-btn" @click="showDatePicker">{{formatDate || 'Please select date'}}
          </cube-button>
          <date-picker ref="datePicker" title="请选择日期" :min="new Date(1960, 1, 1)" :max="new Date()"
            :value="new Date(model.dateValue)" @select="dateSelectHandler">
          </date-picker>
        </cube-form-item>
      </cube-form-group>
      <cube-form-group>
        <cube-button class="submit-btn" type="submit">Submit</cube-button>
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
          sexy: 1,
          dateValue: new Date(1993, 9, 13),
          card: ''
        },
        fields: [{
            type: 'input',
            modelKey: 'name',
            label: '真实姓名',
            props: {
              placeholder: '请输入姓名'
            },
            valid: undefined,
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
              required: true,
              len: 11
            },
            messages: {
              len: '请输入正确的号码格式'
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
              required: true,
              len: 18
            },
            events: {
              blur: (e) => {
                let carded = e.target.value;
                this.model.sexy = carded && carded.charAt(16) % 2 ? 1 : 2;
                console.log(this.model.sexy)
                let year = carded.slice(6, 10);
                let month = Number(carded.slice(10, 12)) -1;
                let day = carded.slice(12, 14);
                this.model.dateValue = new Date(year, month, day);
                console.log(year, month, day, this.model.dateValue)
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
    computed: {
      formatDate: function() {
        let year = this.model.dateValue.getFullYear(),
          month = this.model.dateValue.getMonth() + 1,
          day = this.model.dateValue.getDate();
        return year + '-' + month + '-' + day;
      }
    },
    mounted() {
      // this.model.phone = this.hideMobile();
    },
    methods: {
      submitHandler(e) {
        e.preventDefault()
        console.log('submit', e)
        console.log(this.model)
        // let temp = JSO
        this.$createDialog({
        type: 'warn',
        content: `Selected Item: <br/> - date: <span style="display: block; width: 100%; word-break:break-all;">${JSON.stringify(this.model)}</span> <br/> }`,
        icon: 'cubeic-alert'
      }).show()
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
        this.model.dateValue = date;
      },
      hideMobile() {
        return this.model.phone.replace(/(\d{3})\d{4}(\d{4})/, '$1****$2');
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

  .cube-validator-msg {
    border: solid 1px red;
    font-size: .75rem;
    display: block
  }

  .date-btn {
    background: transparent;
    text-align: left;
    color: inherit;
    padding-left: 0;
  }

  .submit-btn {
    background: rgb(40, 192, 40);
    width: 85%;
    margin: 0 auto;
    margin-top: 1rem;
    padding: 1rem;
  }

  .cube-dialog-content-def > p {
    width: 80%;
    word-break: break-all;
  }
</style>