<script>
  import supabase from "$lib/db"
  import Todo from "$lib/Todo/Todo.svelte"
  import Title from "$lib/Title.svelte"
  import { onMount } from "svelte"
  import { PlusIcon } from "svelte-feather-icons"
  import { fly } from "svelte/transition"
  import { flip } from "svelte/animate"

  let todos = []
  let newTask = ""

  onMount(async () => {
    await getAllTodos()
  })

  const getAllTodos = async () => {
    try {
      let { data, error } = await supabase
        .from("todos")
        .select("*")
        .order("isComplete", { ascending: true })
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
      getAllTodos()
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

<svelte:head>
  <title>Your todos</title>
</svelte:head>

<section class="container">
  <Title>What to do?</Title>

  <div class="add-todo">
    <input placeholder="Drop the soap..." type="text" bind:value={newTask} />
    <button aria-label="add task" on:click={() => addTodo()}
      ><PlusIcon size="1.3x" /></button
    >
  </div>

  <div class="todos">
    {#each todos as todo (todo.id)}
      <div
        animate:flip={{ duration: 250 }}
        in:fly={{ x: 10, duration: 250 }}
        out:fly={{ x: 10, duration: 250 }}
      >
        <Todo {todo} {updateTodo} {deleteTodo} />
      </div>
    {:else}
      <p>Nothing to do...</p>
    {/each}
  </div>
</section>

<svelte:window on:keypress={handleKeypress} />

<style>
  .add-todo {
    display: grid;
    grid-template-columns: 1fr auto;
    gap: min(1rem, 2vw);
    margin-bottom: 1rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid hsla(0, 0%, 100%, 0.2);
  }

  .todos {
    display: grid;
    gap: min(1rem, 2vw);
  }

  p {
    font-size: 0.8rem;
    color: #fff;
  }

  input,
  button {
    border: none;
    border-radius: 0.25em;
  }

  input {
    padding: 0.5rem 1rem;
    font-size: 0.875rem;
    line-height: 1.5;
  }

  button {
    height: 100%;
    padding: 0 0.6rem;
    background: hsl(147, 51%, 46%);
    color: #fff;
    display: flex;
    align-items: center;
    cursor: pointer;
  }
</style>
