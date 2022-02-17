<script>
  import supabase from "$lib/db"
  import Todo from "$lib/Todo/Todo.svelte"
  import { onMount } from "svelte"

  let todos = []
  let newTask = ""

  onMount(async () => {
    await getAllTodos()
  })

  const getAllTodos = async () => {
    try {
      let { data, error } = await supabase.from("todos").select("*")
      todos = data
    } catch (error) {
      console.log(error)
    }
  }

  const updateTodo = async todo => {
    try {
      const { data, error } = await supabase
        .from("todos")
        .update({ task: todo.task, isComplete: todo.isComplete })
        .eq("id", todo.id)
    } catch (error) {
      console.log(error)
    }
  }

  const addTodo = async task => {
    try {
      const { data, error } = await supabase
        .from("todos")
        .insert([{ task: newTask }])
      getAllTodos()
      newTask = ""
    } catch (error) {
      console.log(error)
    }
  }

  const deleteTodo = async todo => {
    try {
      const { data, error } = await supabase
        .from("todos")
        .delete()
        .eq("id", todo.id)
      getAllTodos()
    } catch (error) {
      console.log(error)
    }
  }

  const handleKeypress = e => {
    if (e.key === "Enter" && newTask !== "") {
      addTodo()
    }
  }
</script>

<h1>What to do?</h1>

<div class="add-todo">
  <input type="text" bind:value={newTask} />
  <button on:click={() => addTodo()}>Add task</button>
</div>

<div class="todos">
  {#each todos as todo}
    <Todo {todo} {updateTodo} {deleteTodo} />
  {:else}
    <p>Nothing to do...</p>
  {/each}
</div>

<svelte:window on:keypress={handleKeypress} />

<style>
  :global(body) {
    background-color: rgb(46, 50, 73);
  }
</style>
