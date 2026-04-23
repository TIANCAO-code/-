<script setup>
const props = defineProps({
    goodsList: {
        type: Array,
        required: true,
        defaults: []
    }
})
</script>
<template>
    <div class="table-wrapper">
    <table class="table table-striped table-bordered">
        <thead>
            <slot name="header"></slot>
        </thead>
        <tbody>
            <tr v-for="(item, index) in goodsList" :key="item.id">
                <slot name="body" :row="item" :index="index"></slot>
            </tr>
        </tbody>
    </table>
    </div>
</template>

<style scoped>
.table-wrapper {
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(104, 196, 138, 0.08);
  overflow: hidden;
  border: 1px solid #e8f4ec;
}

.fresh-table {
  width: 100%;
  border-collapse: collapse;
  color: #4a6b57;
  font-size: 14px;
  margin: 0;
}

:deep(th) {
  background-color: #f0f9f3;
  color: #3b7b54;
  font-weight: 600;
  padding: 14px 16px;
  text-align: center;
  border-bottom: 2px solid #d4ebd9;
}

:deep(td) {
  padding: 14px 16px;
  border-bottom: 1px solid #eff6f2;
  vertical-align: middle;
  text-align: center;
}

tbody tr {
  transition: background-color 0.3s ease;
}

tbody tr:hover {
  background-color: #f8fdf9;
}
</style>