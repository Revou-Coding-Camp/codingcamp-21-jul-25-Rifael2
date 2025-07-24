# codingcamp-21-jul-25-Rifael2
#codingcamp-21-jul-25-Rifael2 created by GitHub Classroom
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> To-Do List</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .task-card {
            transition: all 0.3s ease;
        }
        .task-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
        }
        .priority-high {
            border-left: 4px solid #ef4444;
        }
        .priority-medium {
            border-left: 4px solid #f59e0b;
        }
        .priority-low {
            border-left: 4px solid #10b981;
        }
        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen">
    <div class="container mx-auto px-4 py-8 max-w-4xl">
        <header class="text-center mb-12">
            <h1 class="text-4xl font-bold text-gray-800 mb-2">to-do list</h1>
            <p class="text-lg text-gray-600">yok nugas</p>
        </header>

        <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
           
            <div class="lg:col-span-1">
                <div class="bg-white rounded-lg shadow p-6 sticky top-4">
                    <h2 class="text-xl font-semibold text-gray-800 mb-4">Add New Task</h2>
                    
                    <form id="todoForm" class="space-y-4">
                        <div>
                            <label for="taskInput" class="block text-sm font-medium text-gray-700 mb-1">Task Description</label>
                            <input type="text" id="taskInput" 
                                   class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500"
                                   placeholder="What needs to be done?" autocomplete="off">
                            <div id="taskError" class="text-red-500 text-sm mt-1 hidden"></div>
                        </div>
                        
                        <div>
                            <label for="taskDate" class="block text-sm font-medium text-gray-700 mb-1">Due Date</label>
                            <input type="date" id="taskDate" 
                                   class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <div id="dateError" class="text-red-500 text-sm mt-1 hidden"></div>
                        </div>
                        
                        <div>
                            <label for="taskPriority" class="block text-sm font-medium text-gray-700 mb-1">Priority</label>
                            <select id="taskPriority" class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500">
                                <option value="low">Low</option>
                                <option value="medium" selected>Medium</option>
                                <option value="high">High</option>
                            </select>
                        </div>
                        
                        <button type="submit" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-medium py-2 px-4 rounded-lg transition duration-200">
                            Add Task
                        </button>
                    </form>
                    
                    <div class="mt-6">
                        <h3 class="text-lg font-semibold text-gray-800 mb-2">Filter Tasks</h3>
                        <div class="space-y-3">
                            <input type="text" id="filterText" placeholder="Search tasks..." 
                                   class="w-full px-4 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500">
                            
                            <div class="flex space-x-2">
                                <select id="filterPriority" class="flex-1 px-4 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    <option value="all">All Priorities</option>
                                    <option value="high">High</option>
                                    <option value="medium">Medium</option>
                                    <option value="low">Low</option>
                                </select>
                                
                                <input type="date" id="filterDate" 
                                       class="flex-1 px-4 py-2 rounded-lg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-blue-500">
                            </div>
                            
                            <button id="clearFilters" class="w-full text-gray-600 hover:text-gray-800 font-medium text-sm py-1 px-3 rounded transition duration-200">
                                Clear Filters
                            </button>
                        </div>
                    </div>
                </div>
            </div>
            
          
            <div class="lg:col-span-2">
                <div class="bg-white rounded-lg shadow p-6">
                    <div class="flex justify-between items-center mb-4">
                        <h2 class="text-xl font-semibold text-gray-800">Your Tasks</h2>
                        <span id="taskCount" class="text-sm text-gray-500">0 tasks</span>
                    </div>
                    
                    <div id="taskList" class="space-y-3">
                       
                        <div id="emptyState" class="text-center py-8">
                            <img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExNGcxNG13YTJkdnB5d3FhNncxNWo0bGY1emswcnBmbzYxdWg0dTRxMyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9cw/7Fg6W6z6TgP0EFvrD5/giphy.gif" alt="Empty clipboard with pencil illustration" class="mx-auto mb-4 rounded-lg">
                            <h3 class="text-lg font-medium text-gray-700">No tasks yet</h3>
                            <p class="text-gray-500">Add your first task to get started!</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

 
    <template id="taskTemplate">
        <div class="task-card bg-white rounded-lg shadow-sm p-4 fade-in priority-{priority}">
            <div class="flex justify-between items-start">
                <div class="flex items-start space-x-3 flex-1 min-w-0">
                    <input type="checkbox" class="mt-1 rounded text-blue-600 focus:ring-blue-500 cursor-pointer">
                    
                    <div class="flex-1 min-w-0">
                        <div class="flex items-center space-x-2">
                            <p class="task-description text-gray-800 font-medium truncate"></p>
                            <span class="priority-badge px-2 py-1 rounded-full text-xs font-medium"></span>
                        </div>
                        <p class="task-date text-sm text-gray-500 mt-1"></p>
                    </div>
                </div>
                
                <button class="delete-task text-red-500 hover:text-red-700 transition duration-200 ml-2">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                    </svg>
                </button>
            </div>
        </div>
    </template>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            
            const todoForm = document.getElementById('todoForm');
            const taskInput = document.getElementById('taskInput');
            const taskDate = document.getElementById('taskDate');
            const taskPriority = document.getElementById('taskPriority');
            const taskList = document.getElementById('taskList');
            const emptyState = document.getElementById('emptyState');
            const filterText = document.getElementById('filterText');
            const filterPriority = document.getElementById('filterPriority');
            const filterDate = document.getElementById('filterDate');
            const clearFilters = document.getElementById('clearFilters');
            const taskCount = document.getElementById('taskCount');
            const taskTemplate = document.getElementById('taskTemplate');
            const taskError = document.getElementById('taskError');
            const dateError = document.getElementById('dateError');
             
            let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
         
            renderTasks();
            updateTaskCount();
            
            todoForm.addEventListener('submit', function(e) {
                e.preventDefault();
                
                const description = taskInput.value.trim();
                const date = taskDate.value;
                const priority = taskPriority.value;
            
                let isValid = true;
                
                if (!description) {
                    taskError.textContent = 'Task description is required';
                    taskError.classList.remove('hidden');
                    isValid = false;
                } else {
                    taskError.classList.add('hidden');
                }
                
                if (!date) {
                    dateError.textContent = 'Due date is required';
                    dateError.classList.remove('hidden');
                    isValid = false;
                } else {
                    dateError.classList.add('hidden');
                }
                
                if (!isValid) return;
                 
                addTask(description, date, priority);
              
                taskInput.value = '';
                taskDate.value = '';
                taskPriority.value = 'medium';
                taskInput.focus();
            });
             
            filterText.addEventListener('input', filterTasks);
            filterPriority.addEventListener('change', filterTasks);
            filterDate.addEventListener('change', filterTasks);
            clearFilters.addEventListener('click', resetFilters);
            
        
            taskList.addEventListener('click', function(e) {
            
                if (e.target.closest('.delete-task')) {
                    const taskElement = e.target.closest('.task-card');
                    const taskId = parseInt(taskElement.dataset.taskId);
                    deleteTask(taskId);
                }
               
                if (e.target.type === 'checkbox') {
                    const taskElement = e.target.closest('.task-card');
                    const taskId = parseInt(taskElement.dataset.taskId);
                    toggleTaskCompleted(taskId);
                }
            });
           
            function addTask(description, date, priority) {
                const newTask = {
                    id: Date.now(),
                    description,
                    date,
                    priority,
                    completed: false,
                    createdAt: new Date().toISOString()
                };
                
                tasks.push(newTask);
                saveTasks();
                renderTasks();
                updateTaskCount();
            }
            
            function deleteTask(taskId) {
                tasks = tasks.filter(task => task.id !== taskId);
                saveTasks();
                renderTasks();
                updateTaskCount();
            }
            
            function toggleTaskCompleted(taskId) {
                const task = tasks.find(task => task.id === taskId);
                if (task) {
                    task.completed = !task.completed;
                    saveTasks();
                    renderTasks();
                }
            }
            
            function saveTasks() {
                localStorage.setItem('tasks', JSON.stringify(tasks));
            }
            
            function renderTasks(filteredTasks = tasks) {
                taskList.innerHTML = '';
                
                if (filteredTasks.length === 0) {
                    emptyState.classList.remove('hidden');
                    taskList.appendChild(emptyState);
                    return;
                }
                
                emptyState.classList.add('hidden');
             
                filteredTasks.sort((a, b) => {
                
                    if (a.completed !== b.completed) {
                        return a.completed ? 1 : -1;
                    }
                     
                    const dateA = new Date(a.date);
                    const dateB = new Date(b.date);
                    if (dateA < dateB) return -1;
                    if (dateA > dateB) return 1;
              
                    const priorityOrder = { high: 1, medium: 2, low: 3 };
                    return priorityOrder[a.priority] - priorityOrder[b.priority];
                });
                
                filteredTasks.forEach(task => {
                    const taskElement = document.importNode(taskTemplate.content, true);
                    const taskCard = taskElement.querySelector('.task-card');
                    const taskDescription = taskElement.querySelector('.task-description');
                    const taskDateElement = taskElement.querySelector('.task-date');
                    const priorityBadge = taskElement.querySelector('.priority-badge');
                    const checkbox = taskElement.querySelector('input[type="checkbox"]');
                    
                    taskCard.dataset.taskId = task.id;
                    taskDescription.textContent = task.description;
                     
                    const dateObj = new Date(task.date);
                    const formattedDate = dateObj.toLocaleDateString('en-US', { 
                        weekday: 'short', 
                        month: 'short', 
                        day: 'numeric',
                        year: 'numeric'
                    });
                    taskDateElement.textContent = `Due: ${formattedDate}`;
                     
                    priorityBadge.textContent = task.priority.charAt(0).toUpperCase() + task.priority.slice(1);
                    priorityBadge.className += ` ${
                        task.priority === 'high' ? 'bg-red-100 text-red-800' :
                        task.priority === 'medium' ? 'bg-yellow-100 text-yellow-800' :
                        'bg-green-100 text-green-800'
                    } px-2 py-1 rounded-full text-xs font-medium`; 

                    if (task.completed) {
                        taskDescription.classList.add('line-through', 'text-gray-400');
                        taskDateElement.classList.add('line-through', 'text-gray-400');
                        checkbox.checked = true;
                    } else {
                        taskDescription.classList.remove('line-through', 'text-gray-400');
                        taskDateElement.classList.remove('line-through', 'text-gray-400');
                        checkbox.checked = false;
                    }
                    
                    taskList.appendChild(taskElement);
                });
            }
            
            function filterTasks() {
                const searchText = filterText.value.toLowerCase();
                const selectedPriority = filterPriority.value;
                const selectedDate = filterDate.value;
                
                let filteredTasks = tasks;
                 
                if (searchText) {
                    filteredTasks = filteredTasks.filter(task => 
                        task.description.toLowerCase().includes(searchText)
                    );
                }
                 
                if (selectedPriority !== 'all') {
                    filteredTasks = filteredTasks.filter(task => 
                        task.priority === selectedPriority
                    );
                }
                 
                if (selectedDate) {
                    filteredTasks = filteredTasks.filter(task => 
                        task.date === selectedDate
                    );
                }
                
                renderTasks(filteredTasks);
                updateTaskCount(filteredTasks);
            }
            
            function resetFilters() {
                filterText.value = '';
                filterPriority.value = 'all';
                filterDate.value = '';
                renderTasks();
                updateTaskCount();
            }
            
            function updateTaskCount(filteredTasks = tasks) {
                const totalTasks = tasks.length;
                const completedTasks = tasks.filter(task => task.completed).length;
                const filteredCount = filteredTasks.length;
                
                if (filteredTasks !== tasks) {
                    taskCount.textContent = `${filteredCount} of ${totalTasks} tasks`;
                } else {
                    taskCount.textContent = `${totalTasks} tasks (${completedTasks} completed)`;
                }
            }
             
            const today = new Date().toISOString().split('T')[0];
            taskDate.min = today;
            filterDate.min = today;
        });
    </script>
</body>
</html>
