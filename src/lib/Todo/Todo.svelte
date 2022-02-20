<script>
  export let todo, updateTodo, deleteTodo
  import { Trash2Icon } from "svelte-feather-icons"
</script>

<div class="todo" class:completed={todo.isComplete}>
  <div class="task-container">
    <input
      class="checkbox"
      type="checkbox"
      checked={todo.isComplete}
      name="todo checkbox"
      id="todo-{todo.id}"
      on:change={e => {
        todo.isComplete = e.currentTarget.checked
        updateTodo(todo)
      }}
    />
    <label for="todo-{todo.id}">
      <span class="screenreader">Todo checkbox</span></label
    >

    <input
      class="task"
      type="text"
      name="todo text"
      aria-label="todo text"
      value={todo.task}
      on:input={e => {
        todo.task = e.currentTarget.value
        updateTodo(todo)
      }}
    />
  </div>

  <button aria-label="delete" on:click={() => deleteTodo(todo)}
    ><Trash2Icon size="1.3x" /></button
  >
</div>

<style>
  .todo {
    display: grid;
    grid-template-columns: 1fr auto;
    align-items: center;
  }

  .task-container {
    background-color: #fff;
    display: grid;
    grid-template-columns: auto 1fr;
    border-radius: 0.25em;
    padding-left: 0.5rem;
  }

  input,
  button {
    border: none;
    border-radius: 0.25em;
  }

  button {
    height: 100%;
    padding: 0 0.6rem;
    background: hsl(335, 67%, 47%);
    color: #fff;
    display: flex;
    align-items: center;
    margin-left: min(1rem, 2vw);
    cursor: pointer;
  }
  .task {
    padding: 0.5rem;
    font-size: 0.875rem;
    line-height: 1.5;
    background-color: #fff;
    margin-left: 0.1rem;
  }

  .task:focus {
    outline: none;
  }
  .completed [type="text"] {
    text-decoration: line-through;
    opacity: 0.8;
  }

  .screenreader {
    border: 0;
    clip: rect(0 0 0 0);
    height: 1px;
    margin: -1px;
    overflow: hidden;
    padding: 0;
    position: absolute;
    width: 1px;
  }

  input[type="checkbox"].checkbox {
    opacity: 0;
    position: absolute;
    top: -9999px;
    left: -9999px;
  }

  input[type="checkbox"].checkbox + label {
    position: relative;
    display: flex;
    align-items: center;
    font-size: 1.5em;
    cursor: pointer;
  }

  input[type="checkbox"].checkbox + label::before {
    content: "";
    width: 1em;
    height: 1em;
    border-radius: 99em;
    border: 0.1em solid rgb(46, 50, 73);
    background-color: rgba(46, 50, 73, 0);
    box-sizing: border-box;
    transition: background-color 0.1s ease-in-out;
  }

  input[type="checkbox"]:checked.checkbox + label::before {
    content: "";
    width: 1em;
    height: 1em;
    border-color: rgb(46, 50, 73);
    background-color: rgb(46, 50, 73);
    box-sizing: border-box;
  }

  input[type="checkbox"]:checked.checkbox + label::after {
    position: absolute;
    content: "";
    width: 0.9em;
    height: 0.9em;
    left: 0.05em;
    background-image: url(/static/check.svg);
    background-repeat: no-repeat;
    background-size: 0.9em;
  }

  input[type="checkbox"]:active {
    background: none;
    background-color: none;
  }
</style>
