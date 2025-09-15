# NAME : NAVEEN KUMAR T
# REG NO : 212223220067
# Ex03 To-Do List using JavaScript

## AIM :
To create a To-do Application with all features using JavaScript.

## ALGORITHM :
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM :

### INDEX.HTML 
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Naveen Kumar T . To-Do List</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header class="navbar">
        <a href="index.html">My To-Do List</a>
        <a href="about.html">About This App</a>
    </header>

    <div class="container">
        <h1> To-Do List 📝</h1>
        <div class="input-area">
            <input type="text" id="taskInput" placeholder="Add a new task...">
            <button id="addTaskBtn">Add Task</button>
        </div>
        <div class="list-container">
            <div class="list-column todo-list" ondragover="allowDrop(event)" ondrop="drop(event, 'todo')">
                <h2>To-Do</h2>
                <ul id="todo-ul"></ul>
            </div>
            <div class="list-column completed-list" ondragover="allowDrop(event)" ondrop="drop(event, 'completed')">
                <h2>Completed</h2>
                <ul id="completed-ul"></ul>
            </div>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

### STYLE.CSS 
```
body {
    font-family: Arial, sans-serif;
    background-color: #f0f2f5;
    margin: 0;
    color: #333;
}

.navbar {
    background-color: #007bff;
    padding: 15px 30px;
    display: flex;
    justify-content: flex-start;
    gap: 20px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.navbar a {
    color: white;
    text-decoration: none;
    font-size: 18px;
    font-weight: bold;
    transition: color 0.3s ease;
}

.navbar a:hover {
    color: #cce5ff;
}

.container {
    background: #fff;
    padding: 30px;
    border-radius: 15px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
    width: 90%;
    max-width: 800px;
    margin: 40px auto;
    text-align: center;
}

h1 {
    color: #333;
}

.input-area {
    display: flex;
    justify-content: center;
    margin-bottom: 20px;
}

#taskInput {
    padding: 12px;
    border: 1px solid #ddd;
    border-radius: 8px;
    width: 70%;
    font-size: 16px;
}

#addTaskBtn {
    padding: 12px 20px;
    border: none;
    background-color: #007bff;
    color: white;
    border-radius: 8px;
    margin-left: 10px;
    cursor: pointer;
    font-size: 16px;
    transition: background-color 0.3s ease;
}

#addTaskBtn:hover {
    background-color: #0056b3;
}

.list-container {
    display: flex;
    justify-content: space-around;
    gap: 20px;
}

.list-column {
    background-color: #f9f9f9;
    padding: 20px;
    border-radius: 10px;
    width: 48%;
    min-height: 300px;
    box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.05);
}

.list-column h2 {
    margin-top: 0;
    color: #555;
    border-bottom: 2px solid #ddd;
    padding-bottom: 10px;
}

ul {
    list-style-type: none;
    padding: 0;
    margin-top: 20px;
}

li {
    background: #fff;
    padding: 15px;
    border-radius: 8px;
    margin-bottom: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: grab;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

li:hover {
    transform: translateY(-3px);
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.delete-btn {
    background: #e74c3c;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.2s ease;
}

.delete-btn:hover {
    background: #c0392b;
}

.completed-list li {
    background-color: #d4edda;
    color: #155724;
}

.about-page ul {
    list-style-type: disc;
    text-align: left;
    margin-left: 20px;
}

.about-page li {
    cursor: default;
    background-color: transparent;
    border: none;
    box-shadow: none;
    transform: none;
    padding: 5px 0;
}

.back-link {
    display: inline-block;
    margin-top: 20px;
    padding: 10px 20px;
    background-color: #6c757d;
    color: white;
    text-decoration: none;
    border-radius: 5px;
    transition: background-color 0.3s ease;
}

.back-link:hover {
    background-color: #5a6268;
}
```

### SCRIPT.JSS
```
document.addEventListener('DOMContentLoaded', () => {
    const taskInput = document.getElementById('taskInput');
    const addTaskBtn = document.getElementById('addTaskBtn');
    const todoUl = document.getElementById('todo-ul');
    const completedUl = document.getElementById('completed-ul');

    // Load tasks from Local Storage
    let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    renderTasks();

    addTaskBtn.addEventListener('click', addTask);
    taskInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            addTask();
        }
    });

    function addTask() {
        const taskText = taskInput.value.trim();
        if (taskText === '') {
            alert('Please enter a task.');
            return;
        }

        const newTask = {
            id: Date.now(),
            text: taskText,
            status: 'todo'
        };

        tasks.push(newTask);
        saveTasks();
        renderTasks();
        taskInput.value = '';
    }

    function renderTasks() {
        todoUl.innerHTML = '';
        completedUl.innerHTML = '';

        tasks.forEach(task => {
            const li = document.createElement('li');
            li.setAttribute('data-id', task.id);
            li.setAttribute('draggable', 'true');
            li.innerHTML = `
                <span>${task.text}</span>
                <button class="delete-btn">🗑️</button>
            `;

            if (task.status === 'completed') {
                completedUl.appendChild(li);
            } else {
                todoUl.appendChild(li);
            }

            // Add event listeners for drag and delete
            li.querySelector('.delete-btn').addEventListener('click', () => deleteTask(task.id));
            li.addEventListener('dragstart', dragStart);
        });
    }

    function deleteTask(id) {
        tasks = tasks.filter(task => task.id !== id);
        saveTasks();
        renderTasks();
    }

    function saveTasks() {
        localStorage.setItem('tasks', JSON.stringify(tasks));
    }

    // Drag and Drop Functions
    function dragStart(e) {
        e.dataTransfer.setData('text/plain', e.target.dataset.id);
    }

    window.allowDrop = function(e) {
        e.preventDefault();
    }

    window.drop = function(e, status) {
        e.preventDefault();
        const taskId = e.dataTransfer.getData('text/plain');
        
        const task = tasks.find(t => t.id == taskId);
        if (task) {
            task.status = status;
            saveTasks();
            renderTasks();
        }
    }
});
```

### ABOUT.HTML
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About This App</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header class="navbar">
        <a href="index.html">My To-Do List</a>
        <a href="about.html">About This App</a>
    </header>
    <div class="container about-page">
        <h1>About This App</h1>
        <p>This is a  to-do list application created By Naveen Kumar T.</p>
        <p>It features a clean, simple design with the following functionalities:</p>
        <ul>
            <li><strong>Persistent Storage:</strong> Your tasks are saved automatically using your browser's local storage, so they'll be here the next time you visit.</li>
            <li><strong>Drag-and-Drop:</strong>Easily move tasks from the "To-Do" list to the "Completed" list by dragging them.</li>
            <li><strong>Two-Column Layout:</strong>A clear visual separation between active tasks and completed ones.</li>
        </ul>
        <a href="index.html" class="back-link">Back to the To-Do List</a>
    </div>
</body>
</html>
```
## OUTPUT :
<img width="1913" height="955" alt="Screenshot 2025-09-15 105121" src="https://github.com/user-attachments/assets/74465ba8-ebe3-46e6-a513-142be20ee449" />

<img width="1911" height="936" alt="Screenshot 2025-09-15 104554" src="https://github.com/user-attachments/assets/c555a743-cc48-4aeb-a72d-fb7b7db66abe" />

## RESULT :
The program for creating To-do list using JavaScript is executed successfully.
