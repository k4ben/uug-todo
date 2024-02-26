<script>
    let todos = [];
  
    async function fetchTodos() {
      const response = await fetch('http://localhost:8080/todo');
      todos = await response.json();
    }
  
    async function addTodo(title) {
      await fetch('http://localhost:8080/todo', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ title })
      });
      fetchTodos(); // Refresh todos after adding
    }
  
    async function deleteTodo(id) {
      await fetch(`http://localhost:8080/todo/${id}`, {
        method: 'DELETE'
      });
      fetchTodos(); // Refresh todos after deletion
    }
  
    fetchTodos(); // Initial fetch when component is mounted
  </script>
  
  <main class="container">
    <h1>Todo List</h1>
  
  <ul>
    {#each todos as todo (todo.id)}
      <li>{todo.title} <button on:click={() => deleteTodo(todo.id)}>Delete</button></li>
    {/each}
  </ul>
  
  <form on:submit|preventDefault={(event) => {
    const title = event.target.title.value;
    addTodo(title);
    event.target.title.value = '';
  }}>
    <input type="text" name="title" placeholder="Add new todo" required />
    <button type="submit">Add</button>
  </form>
  </main>