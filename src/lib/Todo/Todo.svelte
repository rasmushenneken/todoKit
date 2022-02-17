<script>
  export let todo, updateTodo, deleteTodo
</script>

<div class="todo" class:completed={todo.isComplete}>
  <input
    type="checkbox"
    checked={todo.isComplete}
    name=""
    id=""
    on:change={e => {
      todo.isComplete = e.currentTarget.checked
      updateTodo(todo)
    }}
  />
  <input
    type="text"
    value={todo.task}
    on:input={e => {
      todo.task = e.currentTarget.value
      updateTodo(todo)
    }}
  />
  <button on:click={() => deleteTodo(todo)}>Delete</button>
</div>

<style>
  .todo {
    display: grid;
    grid-template-columns: auto 1fr auto;
  }
  .completed {
    opacity: 0.5;
  }
</style>
