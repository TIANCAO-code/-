<script setup>
import { ref } from 'vue';
import MyTable from './components/MyTable.vue'
import * as XLSX from 'xlsx';
import AuthPage from './components/AuthPage.vue';

const isLoggedIn = ref(false);
const isLoginTab = ref(true);

const handleLogin = () => {
  isLoggedIn.value = true;
};

const fileInput = ref(null);
const onImportClick = () => {
  fileInput.value.click();
};

const handleFileUpload = (e) => {
  const file = e.target.files[0];
  if (!file) return;

  const reader = new FileReader();
  reader.onload = (ev) => {
    const data = ev.target.result;
    const workbook = XLSX.read(data, { type: 'binary' });
    const sheetName = workbook.SheetNames[0];
    const sheet = workbook.Sheets[sheetName];
    const json = XLSX.utils.sheet_to_json(sheet);

    const newGoods = json.map((item, index) => ({
      id: goodsList.value.length + index + 1,
      goods_name: item['商品名称'] || '未知商品',
      goods_price: String(item['价格'] || '0'),
      tags: item['标签'] ? item['标签'].split(',') : [],
      inputVisible: false,
      inputValue: ''
    }));

    goodsList.value.push(...newGoods);
    e.target.value = '';
  };
  reader.readAsBinaryString(file);
};
const goodsList = ref([
  {
    id: 1,
    goods_name: '夏季专柜同款女鞋',
    goods_price: '298',
    tags: ['舒适', '透气'],
    inputVisible: false,
    inputValue: ''
  },
  {
    id: 2,
    goods_name: '冬季保暖女士休闲雪地靴 舒适加绒防水短靴 防滑棉鞋',
    goods_price: '89',
    tags: ['保暖', '防滑'],
    inputVisible: false,
    inputValue: ''
  },
  {
    id: 3,
    goods_name: '秋冬新款女士毛衣 套头宽松针织衫 简约上衣',
    goods_price: '199',
    tags: ['秋冬', '毛衣'],
    inputVisible: false,
    inputValue: ''
  },
  {
    id: 4,
    goods_name: '2023春秋装新款大码女装 衬衫 上衣',
    goods_price: '19',
    tags: ['雪纺衫', '打底'],
    inputVisible: false,
    inputValue: ''
  },
  {
    id: 5,
    goods_name: '长款长袖圆领女士毛衣 2022秋款新款假两件连衣裙',
    goods_price: '178',
    tags: ['圆领', '连衣裙'],
    inputVisible: false,
    inputValue: ''
  }
])
const onRemove = id => {
  goodsList.value = goodsList.value.filter(item => item.id != id)
}
const vFocus = (el) => { el.focus() }
const onInputConfig = row => {
  const val = row.inputValue
  row.inputValue = ''
  row.inputVisible = false
  if (!val || row.tags.indexOf(val) !== -1) {
    return
  }
  row.tags.push(val)
}
</script>

<template>
  <div class="app-container">
    <AuthPage v-if="!isLoggedIn" @login-success="handleLogin" />

    <div v-else>
      <div class="header-actions">
        <h4>商品管理</h4>
        <div>
          <input type="file" ref="fileInput" @change="handleFileUpload" accept=".xlsx, .xls" style="display: none" />
          <button class="btn btn-import" @click="onImportClick">
            <i class="icon-upload"></i> 导入 Excel
          </button>
          <button class="btn btn-outline-secondary btn-sm" style="margin-left:10px" @click="isLoggedIn = false">退出</button>
        </div>
      </div>

      <MyTable :goodsList="goodsList">
        <template v-slot:header>
          <th scope="col">#</th>
          <th scope="col">商品名称</th>
          <th scope="col">价格</th>
          <th scope="col">标签</th>
          <th scope="col" colspan="3">操作</th>
        </template>
        
        <template #body="{ row, index }">
          <td>{{ index + 1 }}</td>
          <td>{{ row.goods_name }}</td>
          <td>￥{{ row.goods_price }}</td>
          <td>{{ row.tags }}</td>
          <td><button type="button" class="btn btn-outline-danger btn-sm" @click="onRemove(row.id)">删除</button></td>
          <td><span class="btn btn-outline-dark" v-for="item in row.tags" :key="item">{{ item }}</span></td>
          <td>
            <input type="text" v-if="row.inputVisible" class="form-control form-control-sm ipt-tag" v-focus v-model="row.inputValue" @blur="onInputConfig(row)" @keyup.enter="onInputConfig(row)" />
            <button class="btn btn-outline-primary rounded-pill" v-else @click="row.inputVisible = true">+Tag</button>
          </td>
        </template>
      </MyTable>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.app-container {
  max-width: 1100px;
  margin: 40px auto;
  padding: 30px;
  background: #fbfdfc; 
  border-radius: 16px;
  box-shadow: 0 6px 24px rgba(99, 192, 137, 0.08);
  font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
}

h4 {
  color: #3b7b54; 
  text-align: center;
  margin-bottom: 24px;
  font-size: 24px;
  font-weight: 600;
  letter-spacing: 1px;
}

.btn {
  display: inline-block;
  padding: 6px 14px;
  border-radius: 6px;
  font-size: 13px;
  cursor: pointer;
  transition: all 0.3s ease;
  border: 1px solid transparent;
  outline: none;
}

.btn-outline-danger {
  color: #d86868;
  background-color: #fff;
  border-color: #fadada;
}
.btn-outline-danger:hover {
  background-color: #fff2f2;
  border-color: #d86868;
}

.btn-outline-dark {
  color: #4da974;
  background-color: #eaf6ef;
  border-color: #c8e6d3;
  margin: 2px 4px;
  border-radius: 12px;
  padding: 4px 10px;
  font-size: 12px;
}

.btn-outline-primary {
  color: #fff;
  background-color: #63c089;
  border-color: #63c089;
  box-shadow: 0 4px 10px rgba(99, 192, 137, 0.25);
}
.btn-outline-primary:hover {
  background-color: #4ea872;
  border-color: #4ea872;
  box-shadow: 0 4px 12px rgba(78, 168, 114, 0.35);
}
.rounded-pill {
  border-radius: 20px !important;
}

.form-control {
  border: 1px solid #c8e6d3;
  border-radius: 6px;
  padding: 4px 10px;
  color: #4a6b57;
  transition: all 0.3s ease;
  background-color: #fff;
}
.form-control:focus {
  border-color: #63c089;
  box-shadow: 0 0 0 3px rgba(99, 192, 137, 0.15);
  outline: none;
}
.ipt-tag {
  margin-right: 8px;
}

.header-actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  border-bottom: 2px dashed #eaf6ef;
  padding-bottom: 15px;
}

.btn-import {
  background-color: #ffffff;
  color: #4da974;
  border: 1.5px solid #63c089;
  border-radius: 8px;
  padding: 8px 16px;
  display: flex;
  align-items: center;
  gap: 6px;
  font-weight: 500;
}

.btn-import:hover {
  background-color: #f0f9f3;
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(99, 192, 137, 0.15);
}

/* 如果需要显示 AuthPage 页面，只需在 App.vue 顶部导入并在 template 中使用即可 */
</style>
