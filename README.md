#include <iostream>
#include <string>
#include <ctime>

using namespace std;

enum Priority {
    LOW,
    MEDIUM,
    HIGH
};

struct Task {
    string description;
    Priority priority;
    string date;
    bool completed;
    Task* next; // Pointer to the next task in the linked list

    Task(string desc, Priority pri, string dt) : description(desc), priority(pri), date(dt), completed(false), next(nullptr) {}
};

class TaskManager {
private:
    Task* head; // Pointer to the head of the linked list

public:
    TaskManager() : head(nullptr) {}

    void addTask(string description, Priority priority, string date) {
        Task* newTask = new Task(description, priority, date);
        // If the linked list is empty, set the new task as the head
        if (head == nullptr) {
            head = newTask;
        } else {
            // Otherwise, find the last task and append the new task to the end
            Task* current = head;
            while (current->next != nullptr) {
                current = current->next;
            }
            current->next = newTask;
        }
    }

    void markTaskCompleted(int index) {
        Task* current = head;
        int i = 0;
        // Traverse the linked list to the specified index
        while (current != nullptr && i < index) {
            current = current->next;
            i++;
        }
        // If the index is valid, mark the task as completed
        if (current != nullptr) {
            current->completed = true;
        }
    }

    void displayTasks() {
        cout << "Tasks:" << endl;
        Task* current = head;
        while (current != nullptr) {
            cout << "Description: " << current->description << " | Priority: " << current->priority << " | Date: " << current->date << " | Completed: " << (current->completed ? "Yes" : "No") << endl;
            current = current->next;
        }
    }

    // Destructor to free memory
    ~TaskManager() {
        Task* current = head;
        while (current != nullptr) {
            Task* next = current->next;
            delete current;
            current = next;
        }
    }
};

int main() {
    TaskManager taskManager;
    taskManager.addTask("Task 1", HIGH, "2024-01-31");
    taskManager.addTask("Task 2", LOW, "2024-02-01");
    taskManager.addTask("Task 3", MEDIUM, "2024-02-02");

    taskManager.displayTasks();
    taskManager.markTaskCompleted(1);
    taskManager.displayTasks();

    return 0;
}
