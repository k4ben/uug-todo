<script>
    import { onMount } from "svelte";

  let todos = [];

  async function fetchTodos() {
    const response = await fetch('http://localhost:8080/todo');
    todos = await response.json();
  }

  async function handleSubmit(event) {
    addTodo(event.target.title.value);
    event.target.title.value = '';
  }

  async function addTodo(title) {
    await fetch('http://localhost:8080/todo', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({ title })
    });
    fetchTodos();
  }

  async function deleteTodo(id) {
    await fetch(`http://localhost:8080/todo/${id}`, {
      method: 'DELETE'
    });
    fetchTodos();
  }

  onMount(()=> fetchTodos());
</script>
<h2>Todo List</h2>

  <form on:submit={handleSubmit}>
    <input type="text" name="title" class="input input-primary" placeholder="Add new todo" />
    <button type="submit" class="btn btn-primary">Add</button>
  </form>

  <br/>

  {#each todos as todo (todo.id)}
  <div style="margin-bottom: 5px"><button on:click={() => deleteTodo(todo.id)}>X</button> {todo.title}</div>
  {/each}
  {#if todos.length == 0}
  <div>No tasks.</div>
  {/if}