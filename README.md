# Coding-Raja-Technology-Internship
import json
import os
from datetime import datetime

DATA_FILE = "tasks.json"

# Initialize the task list
tasks = []

def load_tasks():
    global tasks
    if os.path.exists(DATA_FILE):
        try:
            with open(DATA_FILE, 'r') as file:
                tasks_data = json.load(file)
                # Validate tasks data
                for task in tasks_data:
                    if 'description' in task and 'priority' in task and 'due_date' in task and 'completed' in task:
                        tasks.append(task)
                    else:
                        print("Invalid task data found and skipped:", task)
        except json.JSONDecodeError:
            print("Error loading tasks: JSON file is corrupted.")
            tasks = []

def save_tasks():
    with open(DATA_FILE, 'w') as file:
        json.dump(tasks, file, indent=4)

def add_task(description, priority, due_date):
    task = {
        "description": description,
        "priority": priority,
        "due_date": due_date,
        "completed": False
    }
    tasks.append(task)
    save_tasks()

def remove_task(index):
    if 0 <= index < len(tasks):
        del tasks[index]
        save_tasks()

def mark_task_completed(index):
    if 0 <= index < len(tasks):
        tasks[index]['completed'] = True
        save_tasks()

def list_tasks():
    if not tasks:
        print("No tasks to show.")
        return
    for i, task in enumerate(tasks):
        try:
            status = "Done" if task['completed'] else "Pending"
            print(f"{i + 1}. {task['description']} [Priority: {task['priority']}, Due: {task['due_date']}, Status: {status}]")
        except KeyError as e:
            print(f"Error displaying task {i + 1}: Missing key {e}")

def show_menu():
    print("\nTo-Do List Application")
    print("1. Add Task")
    print("2. Remove Task")
    print("3. Mark Task as Completed")
    print("4. List Tasks")
    print("5. Exit")

def main():
    load_tasks()
    
    while True:
        show_menu()
        choice = input("Choose an option: ")

        if choice == '1':
            description = input("Enter task description: ")
            priority = input("Enter task priority (high, medium, low): ")
            due_date = input("Enter due date (YYYY-MM-DD): ")
            add_task(description, priority, due_date)
        elif choice == '2':
            list_tasks()
            index = int(input("Enter task number to remove: ")) - 1
            remove_task(index)
        elif choice == '3':
            list_tasks()
            index = int(input("Enter task number to mark as completed: ")) - 1
            mark_task_completed(index)
        elif choice == '4':
            list_tasks()
        elif choice == '5':
            print("Exiting application.")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
