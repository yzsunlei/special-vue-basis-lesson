<template>
<div  id="app">
  <table id="table">
    <thead>
      <th>学号</th>
      <th>姓名</th>
      <th>学院</th>
      <th>籍贯</th>
      <th>爱好</th>
      <th>操作</th>
    </thead>
    <tbody>
      <tr v-for="tr, index in tableData">
        <td>{{tr.number}}</td>
        <td>{{tr.name}}</td>
        <td>{{tr.college}}</td>
        <td>{{tr.location}}</td>
        <td>{{tr.interest}}</td>
        <td>
          <button class="button" @click="modifyData(tr, index)">修改</button>
          <button class="button" @click="deleteData(tr, index)">删除</button>
        </td>
      </tr>
      <tr v-if="tableData.length == 0">
        <td colspan="6" class="empty">暂时还没有数据</td>
      </tr>
      <tr>
        <td colspan="6" class="add">
          <button class="button" @click="addData">我要添加数据</button>
        </td>
      </tr>
    </tbody>
  </table>
  <div id="modal" v-if="showModal">
    <h3 class="title">弹窗表单</h3>
    <form class="form">
      <div class="field">
        <label class="label" for="number">学号：</label>
        <input class="input" type="text" id="number" v-model="formData.number"/>
      </div>
      <div class="field">
        <label class="label" for="name">姓名：</label>
        <input class="input" type="text" id="name" v-model="formData.name"/>
      </div>
      <div class="field">
        <label class="label" for="college">学院：</label>
        <input class="input" type="text" id="college" v-model="formData.college"/>
      </div>
      <div class="field">
        <label class="label" for="location">籍贯：</label>
        <input class="input" type="text" id="location" v-model="formData.location"/>
      </div>
      <div class="field">
        <label class="label" for="interest">爱好：</label>
        <input class="input" type="text" id="interest" v-model="formData.interest"/>
      </div>
    </form>
    <div class="action">
      <button class="button" @click="saveData">保存</button>
      <button class="button" @click="showModal = false;">取消</button>
    </div>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      showModal: false,
      formData: {},
      rowIndex: -1
    }
  },
  methods: {
    addData() {
      this.showModal = true;
    },
    saveData() {
      if (!this.formData.number|| !this.formData.name) {
        return alert('学号和姓名不能为空');
      }

      // rowIndex > -1 表示修改数据
      if (this.rowIndex > -1) {
        this.tableData.splice(this.rowIndex, 1, this.formData);
        this.rowIndex = -1;
      } else {
        this.tableData.push(this.formData);
      }

      this.showModal = false;
      this.formData = {};
    },
    modifyData(tr, index) {
      if (tr && tr.number) {
        this.showModal = true;
        this.formData = Object.assign({}, tr);
        this.rowIndex = index;
      }
    },
    deleteData(tr, index) {
      if (tr && tr.number) {
        var rs = confirm("确定要删除学号为" + tr.number + "的数据");
        if (rs == true) {
          this.tableData.splice(index, 1);
        }
      }
    }
  }
}
</script>

<style>
#app {
  width: 500px;
  margin: 200px auto;
  position: relative;
}
#table {
  width: 500px;
  border: 1px solid #ccc;
  border-collapse: collapse;
}
#table th,
#table td {
  border: 1px solid #ccc;
  padding: 5px 8px;
}
#table .empty {
  text-align: center;
}
#table .add {
  text-align: center;
}
#modal {
  width: 400px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -160px;
  margin-left: -200px;
  background: #fff;
  border: 1px solid #ccc;
}
#modal .title {
  text-align: center;
}
#modal .form {
  text-align: center;
  margin-bottom: 20px;
}
#modal .field {
  margin-bottom: 10px;
}
#modal .action {
  text-align: center;
  margin-bottom: 20px;
}
</style>
