<template>
  <div class="kanban" @click="closeMenus">
    <div
        v-for="(column, colIndex) in columns"
        :key="colIndex"
        class="column"
        @dragover.prevent
        @drop="onDrop(colIndex)"
    >
      <h3>{{ column.name }}</h3>
      <div>
        <div
            v-for="(task, tIndex) in column.tasks"
            :key="task.id"
            class="task"
            draggable="true"
            @dragstart="onDragStart(colIndex, tIndex)"
            @click.stop
        >
          <template v-if="task.editing">
            <textarea
                v-model="task.text"
                :ref="el => setEditInputRef(el, task.id)"
                class="edit-textarea"
            ></textarea>
            <button @click="saveEdit(task)"><img src="/check.svg" alt="Save" /></button>
            <button @click="cancelEdit(task)"><img src="/x.svg" alt="Cancel" /></button>
          </template>
          <template v-else>
            <span>{{ task.text }}</span>
            <button @click.stop="showMenu(task)">⋮</button>
            <div v-if="task.showMenu" class="menu" @click.stop>
              <button @click="editTask(task)">Редактировать</button>
              <button @click="deleteTask(colIndex, tIndex)">Удалить</button>
            </div>
          </template>
        </div>
      </div>
      <button v-if="!column.showInput" @click="showInputField(colIndex)">Добавить</button>
      <div v-if="column.showInput">
        <textarea
            v-model="newTaskText[colIndex]"
            :ref="el => setTaskInputRef(el, colIndex)"
            placeholder="Введите задачу"
            rows="3"
            class="task-textarea"
        ></textarea>
        <button @click="addTask(colIndex)"><img src="/check.svg" alt="Save" /></button>
        <button @click="column.showInput = false"><img src="/x.svg" alt="Cancel" /></button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, reactive, nextTick } from 'vue';

export default {
  setup() {
    const columns = reactive([
      { name: 'На согласовании', tasks: [], showInput: false },
      { name: 'Новые', tasks: [], showInput: false },
      { name: 'В процессе', tasks: [], showInput: false },
      { name: 'Готово', tasks: [], showInput: false },
      { name: 'Доработать', tasks: [], showInput: false }
    ]);

    const newTaskText = ref({});
    const taskInputRefs = ref({});
    const editInputs = ref({});
    let draggedTask = null;
    let draggedFromColumn = null;
    let taskId = 0;

    const showInputField = (index) => {
      columns[index].showInput = true;
      nextTick(() => {
        if (taskInputRefs.value[index]) {
          taskInputRefs.value[index].focus();
        }
      });
    };

    const addTask = (index) => {
      if (newTaskText.value[index]?.trim()) {
        columns[index].tasks.push({ id: taskId++, text: newTaskText.value[index], showMenu: false, editing: false });
        newTaskText.value[index] = '';
      }
      columns[index].showInput = false;
    };

    const deleteTask = (columnIndex, taskIndex) => {
      columns[columnIndex].tasks.splice(taskIndex, 1);
      alert(`Задача удалена из "${columns[columnIndex].name}"`);
    };

    const showMenu = (task) => {
      closeMenus();
      task.showMenu = true;
    };

    const closeMenus = () => {
      columns.forEach(column => column.tasks.forEach(task => (task.showMenu = false)));
    };

    const editTask = (task) => {
      closeMenus();
      task.editing = true;
      nextTick(() => {
        if (editInputs.value[task.id]) {
          editInputs.value[task.id].focus();
        }
      });
    };

    const saveEdit = (task) => {
      task.editing = false;
    };

    const cancelEdit = (task) => {
      task.editing = false;
    };

    const onDragStart = (columnIndex, taskIndex) => {
      draggedTask = columns[columnIndex].tasks[taskIndex];
      draggedFromColumn = columnIndex;
    };

    const onDrop = (columnIndex) => {
      if (draggedTask && draggedFromColumn !== null) {
        columns[draggedFromColumn].tasks = columns[draggedFromColumn].tasks.filter(task => task !== draggedTask);
        columns[columnIndex].tasks.push(draggedTask);
        alert(`Задача перемещена в "${columns[columnIndex].name}"`);
      }
      draggedTask = null;
      draggedFromColumn = null;
    };

    const setTaskInputRef = (el, index) => {
      if (el) {
        taskInputRefs.value[index] = el;
      }
    };

    const setEditInputRef = (el, taskId) => {
      if (el) {
        editInputs.value[taskId] = el;
      }
    };

    return {
      columns,
      newTaskText,
      showInputField,
      addTask,
      deleteTask,
      showMenu,
      closeMenus,
      editTask,
      saveEdit,
      cancelEdit,
      onDragStart,
      onDrop,
      setTaskInputRef,
      setEditInputRef
    };
  }
};
</script>

<style>
.kanban {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 10px;
}
.column {
  width: 220px;
  padding: 10px;
  background: #f4f4f4;
  border-radius: 5px;
  min-height: 300px;
}
.task {
  background: white;
  padding: 5px;
  margin: 5px 0;
  border-radius: 3px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: relative;
  cursor: grab;
}
.task-textarea,
.edit-textarea {
  width: 100%;
  height: 60px;
  resize: none;
  padding: 5px;
}
.menu {
  position: absolute;
  top: 20px;
  right: 0;
  background: white;
  border: 1px solid #ccc;
  padding: 5px;
}
</style>
