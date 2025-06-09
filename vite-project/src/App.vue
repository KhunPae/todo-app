<template>
  <div class="app">
    <h1>My Todo List</h1>

    <!-- กล่อง input รับข้อความสิ่งที่ต้องทำ -->
    <input
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="เพิ่มสิ่งที่ต้องทำ..."
    />
    <!-- เมื่อคลิก เรียกฟังก์ชัน addTodo() -->
    <button @click="addTodo">เพิ่ม</button>

    <!-- แสดงรายการสิ่งที่ต้องทำเป็นรายการ (ul > li) -->
    <ul>
      <li v-for="(todo, index) in todos" :key="index" class="todo-item">
        <div class="todo-left">
          <input type="checkbox" v-model="todo.done" />
          <!-- ข้อความจะขีดทับถ้า todo.done = true -->
          <span :class="{ done: todo.done }">{{ todo.text }}</span>
        </div>
        <button @click="removeTodo(index)">ลบ</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
// import ref สำหรับสร้างตัวแปร reactive (ที่ Vue จะติดตามค่าเปลี่ยนแปลง)
import { ref, watch, onMounted } from "vue";
// newTodo: ตัวแปรที่เก็บข้อความที่ผู้ใช้พิมพ์ลงไป
const newTodo = ref("");
// todos: array ที่เก็บรายการ todo ทั้งหมด
const todos = ref([]);
onMounted(() => {
  const saved = localStorage.getItem("todos");
  if (saved) {
    todos.value = JSON.parse(saved);
  }
});
// บันทึกทุกครั้งที่ todos เปลี่ยน
watch(
  todos,
  (newValue) => {
    localStorage.setItem("todos", JSON.stringify(newValue));
  },
  { deep: true }
);

// ฟังก์ชันเพิ่ม Todo
function addTodo() {
  // ถ้ามีข้อความ (ไม่ว่าง) ให้เพิ่มเข้าไปใน array todos
  if (newTodo.value.trim() !== "") {
    todos.value.push({
      text: newTodo.value.trim(),
      done: false,
    });
    newTodo.value = "";
  }
}
function removeTodo(index) {
  todos.value.splice(index);
}
</script>

<style scoped>
.app {
  max-width: 500px;
  margin: 50px auto;
  text-align: center;
  background: rgb(0, 0, 0);
  color: white; /* ✅ เพิ่มเพื่อให้ข้อความสีขาว */
}

.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
  background-color: #222; /* พื้นหลังเข้มขึ้น */
  color: white;           /* ตัวหนังสือสีขาว */
  padding: 10px;
  border-radius: 10px;
}


.todo-left {
  display: flex;
  align-items: center;
  gap: 10px;
}

.todo-item button {
  background-color: #ff4d4f;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 8px;
  cursor: pointer;
}

input[type="text"] {
  padding: 8px;
  width: 70%;
  margin-right: 5px;
}

button {
  padding: 8px 12px;
  cursor: pointer;
}

.done {
  text-decoration: line-through;
  color: rgba(255, 255, 255, 0.6); /* ✅ สีขาวโปร่งใส */
}
</style>
