<script setup>
import { reactive } from "vue";
import InputNew from "./InputNew.vue";

let boards = reactive([
  {
    id: crypto.randomUUID(),
    name: "tablero 1",
    items: [
      {
        id: crypto.randomUUID(),
        title: "Feature de archivos",
      },
      {
        id: crypto.randomUUID(),
        title: "Resolver bug",
      },
    ],
  },
  {
    id: crypto.randomUUID(),
    name: "tablero 2",
    items: [
      {
        id: crypto.randomUUID(),
        title: "Mandar reporte",
      },
      {
        id: crypto.randomUUID(),
        title: "Code Review",
      },
    ],
  },
]);

function handleNewItem(text, board) {
  board.items.push({
    id: crypto.randomUUID(),
    title: text.value,
  });
  console.log(`Added ${text.value} to board ${board.name} [${board.id}]`);
}

function handleNewBoard() {
  const name = prompt("Name of the board");
  if (!!name) {
    boards.push({
      id: crypto.randomUUID(),
      name: name,
      items: [],
    });
  }
}

function startDrag(evt, board, item) {
  console.log(
    `${item.title} from ${board.name} was grabbed and its being dragged`
  );
  evt.dataTransfer.setData(
    "text/plain",
    JSON.stringify({ boardId: board.id, itemId: item.id })
  );
}

function onDrop(evt, target) {
  const { boardId, itemId } = JSON.parse(
    evt.dataTransfer.getData("text/plain")
  );
  const originBoard = boards.find((item) => item.id === boardId);
  const originItem = originBoard.items.find((item) => item.id === itemId);

  console.log(
    `${originItem.title} from ${originBoard.name} was dropped into ${target.name}`
  );

  target.items.push({ ...originItem });
  originBoard.items = originBoard.items.filter((item) => item !== originItem);
}
</script>

<template>
  <nav>
    <ul>
      <li>
        <a href="#" @click.prevent="handleNewBoard">Create board</a>
      </li>
    </ul>
  </nav>

  <div class="boards-container">
    <div class="boards">
      <!--
        Why is board a parameter of onDrop here?
        Because we are going to remove the element from the original board,
        and put it in the target board

        board here represents the target board
    -->
      <div
        class="board"
        @drop="onDrop($event, board)"
        @dragover.prevent
        @dragenter.prevent
        v-for="board in boards"
        :key="board.id"
      >
        <div>{{ board.name }}</div>
        <InputNew @onNewItem="(text) => handleNewItem(text, board)"></InputNew>
        <div class="items">
          <div
            class="item"
            draggable="true"
            @dragstart="startDrag($event, board, item)"
            v-for="item in board.items"
            :key="item.id"
          >
            {{ item.title }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
nav {
  background-color: black;
  margin-bottom: 10px;
}

nav ul {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
}

nav ul li a {
  display: block;
  padding: 10px;
  color: white;
  text-decoration: none;
}
.boards {
  display: flex;
  gap: 20px;
}

.board {
  background-color: #efefef;
  padding: 10px;
}

.items {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.item {
  background-color: white;
  padding: 10px;
}
</style>
