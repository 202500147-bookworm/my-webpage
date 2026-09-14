[index (3).html](https://github.com/user-attachments/files/32181883/index.3.html)
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>나의 시작 홈페이지</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      min-height: 100vh;
      font-family: "Noto Sans KR", Arial, sans-serif;
      background: linear-gradient(135deg, #eef4ff, #f8f9fc);
      color: #202124;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 32px 16px;
    }
    .container {
      width: min(900px, 100%);
    }
    .greeting {
      text-align: center;
      margin-bottom: 28px;
    }
    .greeting h1 {
      margin: 0 0 8px;
      font-size: clamp(28px, 5vw, 44px);
    }
    .greeting p {
      margin: 0;
      color: #687080;
      font-size: 15px;
    }
    .clock-card {
      text-align: center;
      background: rgba(255,255,255,.85);
      border-radius: 24px;
      padding: 26px;
      box-shadow: 0 12px 35px rgba(40,60,100,.10);
      margin-bottom: 20px;
    }
    #date {
      color: #667085;
      font-size: 17px;
      margin-bottom: 7px;
    }
    #time {
      font-size: clamp(42px, 9vw, 72px);
      font-weight: 700;
      letter-spacing: 2px;
    }
    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }
    .card {
      background: rgba(255,255,255,.9);
      border-radius: 22px;
      padding: 24px;
      box-shadow: 0 12px 35px rgba(40,60,100,.10);
    }
    .card h2 {
      margin: 0 0 18px;
      font-size: 20px;
    }
    .search {
      display: flex;
      gap: 9px;
    }
    .search input {
      min-width: 0;
      flex: 1;
      border: 1px solid #d9dee8;
      border-radius: 12px;
      padding: 13px 14px;
      font-size: 15px;
      outline: none;
    }
    .search input:focus {
      border-color: #7b9cff;
      box-shadow: 0 0 0 3px rgba(123,156,255,.15);
    }
    button {
      border: 0;
      border-radius: 12px;
      padding: 0 17px;
      background: #4f6df5;
      color: white;
      font-weight: 600;
      cursor: pointer;
    }
    button:hover { background: #405bd5; }
    .todo-add {
      display: flex;
      gap: 8px;
      margin-bottom: 15px;
    }
    .todo-add input {
      flex: 1;
      min-width: 0;
      border: 1px solid #d9dee8;
      border-radius: 12px;
      padding: 12px 13px;
      font-size: 14px;
      outline: none;
    }
    .todo-add button { padding: 0 14px; }
    #todoList {
      list-style: none;
      padding: 0;
      margin: 0;
      max-height: 250px;
      overflow-y: auto;
    }
    #todoList li {
      display: flex;
      align-items: center;
      gap: 9px;
      padding: 9px 0;
      border-bottom: 1px solid #edf0f5;
    }
    #todoList li:last-child { border-bottom: 0; }
    #todoList label {
      flex: 1;
      word-break: break-word;
      cursor: pointer;
    }
    #todoList .done {
      text-decoration: line-through;
      color: #98a2b3;
    }
    .delete {
      background: transparent;
      color: #98a2b3;
      padding: 4px 7px;
      font-size: 18px;
    }
    .delete:hover {
      background: #f2f4f7;
      color: #667085;
    }
    .empty {
      color: #98a2b3;
      text-align: center;
      padding: 14px 0;
      font-size: 14px;
    }
    footer {
      text-align: center;
      color: #98a2b3;
      font-size: 12px;
      margin-top: 22px;
    }
    @media (max-width: 680px) {
      .grid { grid-template-columns: 1fr; }
      .card { padding: 20px; }
    }
  </style>
</head>
<body>
  <main class="container">
    <section class="greeting">
      <h1>안녕하세요 👋</h1>
      <p>오늘도 차분하게, 하나씩 시작해 보세요.</p>
    </section>

    <section class="clock-card">
      <div id="date"></div>
      <div id="time"></div>
    </section>

    <section class="grid">
      <div class="card">
        <h2>🔎 Google 검색</h2>
        <form class="search" action="https://www.google.com/search" method="GET" target="_self">
          <input id="searchInput" type="search" name="q" placeholder="검색어를 입력하세요" autocomplete="off">
          <button type="submit">검색</button>
        </form>
      </div>

      <div class="card">
        <h2>✅ 오늘의 할 일</h2>
        <form class="todo-add" id="todoForm">
          <input id="todoInput" type="text" placeholder="할 일을 입력하세요" maxlength="100">
          <button type="submit">추가</button>
        </form>
        <ul id="todoList"></ul>
      </div>
    </section>

    <footer>할 일 목록은 이 브라우저에 자동 저장됩니다.</footer>
  </main>

  <script>
    const dateEl = document.getElementById("date");
    const timeEl = document.getElementById("time");

    function updateClock() {
      const now = new Date();
      dateEl.textContent = now.toLocaleDateString("ko-KR", {
        year: "numeric", month: "long", day: "numeric", weekday: "long"
      });
      timeEl.textContent = now.toLocaleTimeString("ko-KR", {
        hour: "2-digit", minute: "2-digit", second: "2-digit", hour12: false
      });
    }
    updateClock();
    setInterval(updateClock, 1000);

    const todoForm = document.getElementById("todoForm");
    const todoInput = document.getElementById("todoInput");
    const todoList = document.getElementById("todoList");
    let todos = JSON.parse(localStorage.getItem("myStartPageTodos") || "[]");

    function saveTodos() {
      localStorage.setItem("myStartPageTodos", JSON.stringify(todos));
    }

    function renderTodos() {
      todoList.innerHTML = "";
      if (!todos.length) {
        todoList.innerHTML = '<li class="empty">아직 등록된 할 일이 없습니다.</li>';
        return;
      }

      todos.forEach((todo, index) => {
        const li = document.createElement("li");

        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.checked = todo.done;
        checkbox.addEventListener("change", () => {
          todos[index].done = checkbox.checked;
          saveTodos();
          renderTodos();
        });

        const label = document.createElement("label");
        label.textContent = todo.text;
        if (todo.done) label.classList.add("done");
        label.addEventListener("click", () => {
          todos[index].done = !todos[index].done;
          saveTodos();
          renderTodos();
        });

        const deleteBtn = document.createElement("button");
        deleteBtn.className = "delete";
        deleteBtn.type = "button";
        deleteBtn.textContent = "×";
        deleteBtn.title = "삭제";
        deleteBtn.addEventListener("click", () => {
          todos.splice(index, 1);
          saveTodos();
          renderTodos();
        });

        li.append(checkbox, label, deleteBtn);
        todoList.appendChild(li);
      });
    }

    todoForm.addEventListener("submit", (e) => {
      e.preventDefault();
      const text = todoInput.value.trim();
      if (!text) return;
      todos.push({ text, done: false });
      saveTodos();
      renderTodos();
      todoInput.value = "";
      todoInput.focus();
    });

    renderTodos();
  </script>
</body>
</html>
