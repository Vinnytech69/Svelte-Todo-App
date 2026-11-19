<script>
  /** @typedef {{ id: number, title: string, completed: boolean }} Task */

  /** @type {Task[]} */
  let tasks = $state([]);

  let newTask = $state('');

  function addTask() {
    const title = newTask.trim();

    if (title.length === 0) return;

    tasks = [
      ...tasks,
      {
        id: Date.now(),
        title,
        completed: false
      }
    ];

    newTask = '';
  }

  /** @param {number} id */
  function toggleTask(id) {
    tasks = tasks.map((task) => {
      if (task.id === id) {
        return { ...task, completed: !task.completed };
      }
      return task;
    });
  }

  /** @param {number} id */
  function deleteTask(id) {
    tasks = tasks.filter((task) => task.id !== id);
  }

  let completedCount = $derived(
    tasks.filter((task) => task.completed).length
  );

  let pendingCount = $derived(
    tasks.filter((task) => !task.completed).length
  );
</script>

<svelte:head>
  <title>My Todo List</title>
  <meta name="description" content="A simple task management application" />
</svelte:head>

<main>
  <div class="container">

    <header>
      <div class="logo">✓</div>
      <h1>My Todo List</h1>
      <p>Plan your day and get things done.</p>
    </header>

    <section class="stats">
      <div class="stat">
        <h2>{tasks.length}</h2>
        <p>Total Tasks</p>
      </div>

      <div class="stat">
        <h2>{pendingCount}</h2>
        <p>Pending</p>
      </div>

      <div class="stat">
        <h2>{completedCount}</h2>
        <p>Completed</p>
      </div>
    </section>

    <form
      class="add-form"
      onsubmit={(event) => {
        event.preventDefault();
        addTask();
      }}
    >
      <input
        bind:value={newTask}
        placeholder="Enter a new task..."
        aria-label="New task"
      />

      <button type="submit">Add Task</button>
    </form>

    <section class="task-section">
      <h2 class="section-title">My Tasks</h2>

      {#if tasks.length === 0}
        <div class="empty">
          <p>📝</p>
          <h3>No tasks yet</h3>
          <span>Add your first task to get started.</span>
        </div>
      {:else}
        <ul class="task-list">
          {#each tasks as task (task.id)}
            <li class="task-item">
              <label class="task-label">
                <input
                  type="checkbox"
                  checked={task.completed}
                  onchange={() => toggleTask(task.id)}
                />
                <span class:completed={task.completed}>
                  {task.title}
                </span>
              </label>

              <button
                type="button"
                class="delete-btn"
                onclick={() => deleteTask(task.id)}
              >
                Delete
              </button>
            </li>
          {/each}
        </ul>
      {/if}
    </section>

    <footer>
      <p>Stay organised. Stay productive.</p>
    </footer>

  </div>
</main>

<style>
  :global(*) {
    box-sizing: border-box;
  }

  :global(body) {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f1f5f9;
    color: #1e293b;
  }

  main {
    min-height: 100vh;
    padding: 45px 15px;
  }

  .container {
    max-width: 650px;
    margin: 0 auto;
    padding: 30px;
    background: #ffffff;
    border-radius: 16px;
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.08);
  }

  header {
    text-align: center;
    margin-bottom: 30px;
  }

  .logo {
    width: 55px;
    height: 55px;
    margin: 0 auto 15px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 14px;
    background: #2563eb;
    color: white;
    font-size: 30px;
    font-weight: bold;
  }

  header h1 {
    margin: 0 0 10px;
    color: #1e40af;
    font-size: 30px;
  }

  header p {
    color: #64748b;
  }

  .stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    margin-bottom: 30px;
  }

  .stat {
    padding: 18px 8px;
    text-align: center;
    background: #eff6ff;
    border-radius: 10px;
  }

  .stat h2 {
    margin: 0 0 5px;
    color: #2563eb;
    font-size: 25px;
  }

  .stat p {
    margin: 0;
    color: #64748b;
    font-size: 13px;
  }

  .add-form {
    display: flex;
    gap: 10px;
    margin-bottom: 30px;
  }

  .add-form input {
    flex: 1;
    min-width: 0;
    padding: 13px;
    border: 1px solid #cbd5e1;
    border-radius: 8px;
    font-size: 15px;
  }

  button {
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-weight: bold;
  }

  .add-form button {
    padding: 13px 18px;
    background: #2563eb;
    color: white;
  }

  .add-form button:hover {
    background: #1d4ed8;
  }

  .section-title {
    margin-bottom: 15px;
    font-size: 21px;
  }

  .task-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .task-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    padding: 14px;
    margin-bottom: 10px;
    background: #f8fafc;
    border: 1px solid #e2e8f0;
    border-radius: 9px;
  }

  .task-label {
    display: flex;
    align-items: center;
    gap: 10px;
    min-width: 0;
    overflow-wrap: anywhere;
    cursor: pointer;
  }

  .task-label input {
    accent-color: #2563eb;
    flex-shrink: 0;
  }

  .completed {
    color: #94a3b8;
    text-decoration: line-through;
  }

  .delete-btn {
    padding: 9px 12px;
    background: #fee2e2;
    color: #dc2626;
    flex-shrink: 0;
  }

  .delete-btn:hover {
    background: #fecaca;
  }

  .empty {
    padding: 30px 10px;
    text-align: center;
    background: #f8fafc;
    border-radius: 10px;
  }

  .empty p {
    font-size: 30px;
    margin: 0 0 10px;
  }

  .empty h3 {
    margin: 0 0 8px;
  }

  .empty span {
    color: #64748b;
    font-size: 14px;
  }

  footer {
    margin-top: 30px;
    padding-top: 15px;
    border-top: 1px solid #e2e8f0;
    text-align: center;
    color: #64748b;
    font-size: 13px;
  }

  @media (max-width: 600px) {
    main {
      padding: 20px 10px;
    }

    .container {
      padding: 20px 15px;
    }

    .add-form {
      flex-direction: column;
    }

    header h1 {
      font-size: 25px;
    }

    .stats {
      gap: 6px;
    }

    .stat {
      padding: 14px 4px;
    }

    .stat h2 {
      font-size: 21px;
    }

    .stat p {
      font-size: 11px;
    }
  }
</style>
