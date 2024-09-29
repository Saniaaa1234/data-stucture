# data-stucture

import tkinter as tk
from tkinter import messagebox
from collections import defaultdict, deque
import itertools

# Define colors and fonts
BUTTON_COLOR = "lightblue"
LARGE_FONT = ("Helvetica", 12)
SMALL_FONT = ("Helvetica", 10)

# ---------- Stack Implementation ----------
class Stack:
    def __init__(self):
        self.items = []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop() if not self.is_empty() else None

    def peek(self):
        return self.items[-1] if not self.is_empty() else None

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)

stack = Stack()

def stack_operations():
    def push():
        item = entry_item.get()
        if item:
            stack.push(item)
            entry_item.delete(0, tk.END)
            display_stack()

    def pop():
        popped_item = stack.pop()
        if popped_item is not None:
            messagebox.showinfo("Stack", f"Popped: {popped_item}")
        else:
            messagebox.showerror("Stack", "Stack is empty!")
        display_stack()

    def peek():
        top_item = stack.peek()
        if top_item is not None:
            messagebox.showinfo("Stack", f"Top Item: {top_item}")
        else:
            messagebox.showerror("Stack", "Stack is empty!")

    def display_stack():
        result.set(stack.items)

    stack_window = tk.Toplevel(root)
    stack_window.title("Stack Operations")
    stack_window.geometry("400x300")

    tk.Label(stack_window, text="Stack Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(stack_window, text="Enter item to push:", font=SMALL_FONT).pack(pady=5)
    entry_item = tk.Entry(stack_window, font=SMALL_FONT)
    entry_item.pack(pady=5)

    tk.Button(stack_window, text="Push", command=push, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5, padx=10)
    tk.Button(stack_window, text="Pop", command=pop, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5, padx=10)
    tk.Button(stack_window, text="Peek", command=peek, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5, padx=10)

    result = tk.StringVar()
    tk.Label(stack_window, text="Current Stack:", font=SMALL_FONT).pack(pady=10)
    tk.Label(stack_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Singly Linked List Implementation ----------
class SinglyLinkedList:
    class Node:
        def __init__(self, data):
            self.data = data
            self.next = None

    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = self.Node(data)
        new_node.next = self.head
        self.head = new_node

    def delete(self, data):
        current = self.head
        prev = None
        while current:
            if current.data == data:
                if prev:
                    prev.next = current.next
                else:
                    self.head = current.next
                return
            prev = current
            current = current.next

    def traverse(self):
        current = self.head
        elements = []
        while current:
            elements.append(current.data)
            current = current.next
        return elements

singly_linked_list = SinglyLinkedList()

def singly_linked_list_operations():
    def insert():
        data = entry_singly.get()
        if data:
            singly_linked_list.insert(data)
            entry_singly.delete(0, tk.END)
            display_list()

    def delete():
        data = entry_singly.get()
        if data:
            singly_linked_list.delete(data)
            entry_singly.delete(0, tk.END)
            display_list()

    def display_list():
        result.set(singly_linked_list.traverse())

    sll_window = tk.Toplevel(root)
    sll_window.title("Singly Linked List Operations")
    sll_window.geometry("400x300")

    tk.Label(sll_window, text="Singly Linked List Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(sll_window, text="Enter item to insert/delete:", font=SMALL_FONT).pack(pady=5)
    entry_singly = tk.Entry(sll_window, font=SMALL_FONT)
    entry_singly.pack(pady=5)

    tk.Button(sll_window, text="Insert", command=insert, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
    tk.Button(sll_window, text="Delete", command=delete, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    result = tk.StringVar()
    tk.Label(sll_window, text="Current List:", font=SMALL_FONT).pack(pady=10)
    tk.Label(sll_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Doubly Linked List Implementation ----------
class DoublyLinkedList:
    class Node:
        def __init__(self, data):
            self.data = data
            self.next = None
            self.prev = None

    def __init__(self):
        self.head = None

    def insert(self, data):
        new_node = self.Node(data)
        new_node.next = self.head
        if self.head:
            self.head.prev = new_node
        self.head = new_node

    def delete(self, data):
        current = self.head
        while current:
            if current.data == data:
                if current.prev:
                    current.prev.next = current.next
                if current.next:
                    current.next.prev = current.prev
                if current == self.head:  # Move head if needed
                    self.head = current.next
                return
            current = current.next

    def traverse(self):
        current = self.head
        elements = []
        while current:
            elements.append(current.data)
            current = current.next
        return elements

doubly_linked_list = DoublyLinkedList()

def doubly_linked_list_operations():
    def insert():
        data = entry_dll.get()
        if data:
            doubly_linked_list.insert(data)
            entry_dll.delete(0, tk.END)
            display_list()

    def delete():
        data = entry_dll.get()
        if data:
            doubly_linked_list.delete(data)
            entry_dll.delete(0, tk.END)
            display_list()

    def display_list():
        result.set(doubly_linked_list.traverse())

    dll_window = tk.Toplevel(root)
    dll_window.title("Doubly Linked List Operations")
    dll_window.geometry("400x300")

    tk.Label(dll_window, text="Doubly Linked List Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(dll_window, text="Enter item to insert/delete:", font=SMALL_FONT).pack(pady=5)
    entry_dll = tk.Entry(dll_window, font=SMALL_FONT)
    entry_dll.pack(pady=5)

    tk.Button(dll_window, text="Insert", command=insert, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
    tk.Button(dll_window, text="Delete", command=delete, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    result = tk.StringVar()
    tk.Label(dll_window, text="Current List:", font=SMALL_FONT).pack(pady=10)
    tk.Label(dll_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Queue Implementation ----------
class Queue:
    def __init__(self):
        self.items = deque()

    def enqueue(self, item):
        self.items.append(item)

    def dequeue(self):
        return self.items.popleft() if not self.is_empty() else None

    def is_empty(self):
        return len(self.items) == 0

    def size(self):
        return len(self.items)

queue = Queue()

def queue_operations():
    def enqueue():
        item = entry_queue.get()
        if item:
            queue.enqueue(item)
            entry_queue.delete(0, tk.END)
            display_queue()

    def dequeue():
        dequeued_item = queue.dequeue()
        if dequeued_item is not None:
            messagebox.showinfo("Queue", f"Dequeued: {dequeued_item}")
        else:
            messagebox.showerror("Queue", "Queue is empty!")
        display_queue()

    def display_queue():
        result.set(list(queue.items))

    queue_window = tk.Toplevel(root)
    queue_window.title("Queue Operations")
    queue_window.geometry("400x300")

    tk.Label(queue_window, text="Queue Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(queue_window, text="Enter item to enqueue:", font=SMALL_FONT).pack(pady=5)
    entry_queue = tk.Entry(queue_window, font=SMALL_FONT)
    entry_queue.pack(pady=5)

    tk.Button(queue_window, text="Enqueue", command=enqueue, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
    tk.Button(queue_window, text="Dequeue", command=dequeue, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    result = tk.StringVar()
    tk.Label(queue_window, text="Current Queue:", font=SMALL_FONT).pack(pady=10)
    tk.Label(queue_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Priority Queue Implementation ----------
class PriorityQueue:
    def __init__(self):
        self.elements = []

    def push(self, item, priority):
        self.elements.append((priority, item))
        self.elements.sort()  # Sort by priority

    def pop(self):
        return self.elements.pop(0)[1] if self.elements else None

    def is_empty(self):
        return len(self.elements) == 0

priority_queue = PriorityQueue()

def priority_queue_operations():
    def push():
        item = entry_priority.get()
        priority = entry_priority_value.get()
        if item and priority:
            priority_queue.push(item, int(priority))
            entry_priority.delete(0, tk.END)
            entry_priority_value.delete(0, tk.END)
            display_queue()

    def pop():
        popped_item = priority_queue.pop()
        if popped_item is not None:
            messagebox.showinfo("Priority Queue", f"Popped: {popped_item}")
        else:
            messagebox.showerror("Priority Queue", "Priority Queue is empty!")
        display_queue()

    def display_queue():
        result.set([item for priority, item in priority_queue.elements])

    pq_window = tk.Toplevel(root)
    pq_window.title("Priority Queue Operations")
    pq_window.geometry("400x300")

    tk.Label(pq_window, text="Priority Queue Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(pq_window, text="Enter item to push:", font=SMALL_FONT).pack(pady=5)
    entry_priority = tk.Entry(pq_window, font=SMALL_FONT)
    entry_priority.pack(pady=5)

    tk.Label(pq_window, text="Enter priority:", font=SMALL_FONT).pack(pady=5)
    entry_priority_value = tk.Entry(pq_window, font=SMALL_FONT)
    entry_priority_value.pack(pady=5)

    tk.Button(pq_window, text="Push", command=push, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
    tk.Button(pq_window, text="Pop", command=pop, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    result = tk.StringVar()
    tk.Label(pq_window, text="Current Priority Queue:", font=SMALL_FONT).pack(pady=10)
    tk.Label(pq_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Binary Tree Implementation ----------
class BinaryTree:
    class Node:
        def __init__(self, key):
            self.left = None
            self.right = None
            self.value = key

    def __init__(self):
        self.root = None

    def insert(self, key):
        if self.root is None:
            self.root = self.Node(key)
        else:
            self._insert_recursively(self.root, key)

    def _insert_recursively(self, node, key):
        if key < node.value:
            if node.left is None:
                node.left = self.Node(key)
            else:
                self._insert_recursively(node.left, key)
        else:
            if node.right is None:
                node.right = self.Node(key)
            else:
                self._insert_recursively(node.right, key)

    def inorder(self, node):
        return self.inorder(node.left) + [node.value] + self.inorder(node.right) if node else []

binary_tree = BinaryTree()

def binary_tree_operations():
    def insert():
        key = entry_btree.get()
        if key:
            binary_tree.insert(int(key))
            entry_btree.delete(0, tk.END)
            display_tree()

    def display_tree():
        result.set(binary_tree.inorder(binary_tree.root))

    btree_window = tk.Toplevel(root)
    btree_window.title("Binary Tree Operations")
    btree_window.geometry("400x300")

    tk.Label(btree_window, text="Binary Tree Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(btree_window, text="Enter item to insert:", font=SMALL_FONT).pack(pady=5)
    entry_btree = tk.Entry(btree_window, font=SMALL_FONT)
    entry_btree.pack(pady=5)

    tk.Button(btree_window, text="Insert", command=insert, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    result = tk.StringVar()
    tk.Label(btree_window, text="Current Tree (Inorder):", font=SMALL_FONT).pack(pady=10)
    tk.Label(btree_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Graph Implementation ----------
class Graph:
    def __init__(self):
        self.graph = defaultdict(list)

    def add_edge(self, u, v):
        self.graph[u].append(v)
        self.graph[v].append(u)  # For undirected graph

    def bfs(self, start):
        visited = set()
        queue = deque([start])
        result = []

        while queue:
            vertex = queue.popleft()
            if vertex not in visited:
                visited.add(vertex)
                result.append(vertex)
                queue.extend(self.graph[vertex])
        return result

    def dfs(self, start, visited=None):
        if visited is None:
            visited = set()
        visited.add(start)
        result = [start]
        for neighbor in self.graph[start]:
            if neighbor not in visited:
                result.extend(self.dfs(neighbor, visited))
        return result

graph = Graph()

def graph_operations():
    def add_edge():
        u = entry_edge_u.get()
        v = entry_edge_v.get()
        if u and v:
            graph.add_edge(u, v)
            entry_edge_u.delete(0, tk.END)
            entry_edge_v.delete(0, tk.END)
            messagebox.showinfo("Graph", f"Edge added between {u} and {v}")

    def bfs():
        start = entry_bfs.get()
        if start:
            result.set(graph.bfs(start))

    def dfs():
        start = entry_dfs.get()
        if start:
            result.set(graph.dfs(start))

    graph_window = tk.Toplevel(root)
    graph_window.title("Graph Operations")
    graph_window.geometry("400x400")

    tk.Label(graph_window, text="Graph Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(graph_window, text="Add Edge (u, v):", font=SMALL_FONT).pack(pady=5)
    entry_edge_u = tk.Entry(graph_window, font=SMALL_FONT)
    entry_edge_u.pack(pady=5)
    entry_edge_v = tk.Entry(graph_window, font=SMALL_FONT)
    entry_edge_v.pack(pady=5)

    tk.Button(graph_window, text="Add Edge", command=add_edge, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    tk.Label(graph_window, text="BFS Start Node:", font=SMALL_FONT).pack(pady=5)
    entry_bfs = tk.Entry(graph_window, font=SMALL_FONT)
    entry_bfs.pack(pady=5)
    tk.Button(graph_window, text="Run BFS", command=bfs, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    tk.Label(graph_window, text="DFS Start Node:", font=SMALL_FONT).pack(pady=5)
    entry_dfs = tk.Entry(graph_window, font=SMALL_FONT)
    entry_dfs.pack(pady=5)
    tk.Button(graph_window, text="Run DFS", command=dfs, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    result = tk.StringVar()
    tk.Label(graph_window, text="BFS/DFS Result:", font=SMALL_FONT).pack(pady=10)
    tk.Label(graph_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Traveling Salesman Problem (TSP) ----------
def traveling_salesman_problem(points):
    n = len(points)
    min_path = float('inf')
    min_route = []

    for perm in itertools.permutations(range(n)):
        current_cost = 0
        for i in range(n - 1):
            current_cost += ((points[perm[i]][0] - points[perm[i + 1]][0]) ** 2 + 
                              (points[perm[i]][1] - points[perm[i + 1]][1]) ** 2) ** 0.5
        current_cost += ((points[perm[-1]][0] - points[perm[0]][0]) ** 2 + 
                          (points[perm[-1]][1] - points[perm[0]][1]) ** 2) ** 0.5
        if current_cost < min_path:
            min_path = current_cost
            min_route = perm

    return min_route, min_path

def tsp_operations():
    points = []
    
    def add_point():
        try:
            x = int(entry_x.get())
            y = int(entry_y.get())
            points.append((x, y))
            entry_x.delete(0, tk.END)
            entry_y.delete(0, tk.END)
            result.set(f"Points: {points}")

        except ValueError:
            messagebox.showerror("Input Error", "Please enter valid integers for coordinates.")

    def solve_tsp():
        if len(points) < 2:
            messagebox.showerror("TSP Error", "At least 2 points are required.")
            return
        route, cost = traveling_salesman_problem(points)
        result.set(f"Optimal Route: {route}, Cost: {cost:.2f}")

    tsp_window = tk.Toplevel(root)
    tsp_window.title("Traveling Salesman Problem")
    tsp_window.geometry("400x300")

    tk.Label(tsp_window, text="TSP Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(tsp_window, text="Enter Point (x, y):", font=SMALL_FONT).pack(pady=5)
    entry_x = tk.Entry(tsp_window, font=SMALL_FONT)
    entry_x.pack(pady=5)
    entry_y = tk.Entry(tsp_window, font=SMALL_FONT)
    entry_y.pack(pady=5)

    tk.Button(tsp_window, text="Add Point", command=add_point, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
    tk.Button(tsp_window, text="Solve TSP", command=solve_tsp, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    result = tk.StringVar()
    tk.Label(tsp_window, text="Result:", font=SMALL_FONT).pack(pady=10)
    tk.Label(tsp_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Hash Table Implementation ----------
class HashTable:
    def __init__(self):
        self.table = defaultdict(list)

    def insert(self, key, value):
        self.table[key].append(value)

    def delete(self, key):
        if key in self.table:
            del self.table[key]

    def get(self, key):
        return self.table.get(key, [])

hash_table = HashTable()

def hash_table_operations():
    def insert():
        key = entry_hash_key.get()
        value = entry_hash_value.get()
        if key and value:
            hash_table.insert(key, value)
            entry_hash_key.delete(0, tk.END)
            entry_hash_value.delete(0, tk.END)
            display_table()

    def delete():
        key = entry_hash_key.get()
        if key:
            hash_table.delete(key)
            entry_hash_key.delete(0, tk.END)
            display_table()

    def display_table():
        result.set(dict(hash_table.table))

    hash_window = tk.Toplevel(root)
    hash_window.title("Hash Table Operations")
    hash_window.geometry("400x300")

    tk.Label(hash_window, text="Hash Table Operations", font=LARGE_FONT).pack(pady=10)

    tk.Label(hash_window, text="Enter Key:", font=SMALL_FONT).pack(pady=5)
    entry_hash_key = tk.Entry(hash_window, font=SMALL_FONT)
    entry_hash_key.pack(pady=5)

    tk.Label(hash_window, text="Enter Value:", font=SMALL_FONT).pack(pady=5)
    entry_hash_value = tk.Entry(hash_window, font=SMALL_FONT)
    entry_hash_value.pack(pady=5)

    tk.Button(hash_window, text="Insert", command=insert, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
    tk.Button(hash_window, text="Delete", command=delete, font=SMALL_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

    result = tk.StringVar()
    tk.Label(hash_window, text="Current Hash Table:", font=SMALL_FONT).pack(pady=10)
    tk.Label(hash_window, textvariable=result, font=SMALL_FONT).pack(pady=10)

# ---------- Main GUI Setup ----------
root = tk.Tk()
root.title("Data Structures GUI")
root.geometry("600x600")

tk.Label(root, text="Select the Data Structures and Algorithms you want to perform", font=("Helvetica", 16)).pack(pady=20)

# Create buttons for each data structure
tk.Button(root, text="Stack", command=stack_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
tk.Button(root, text="Singly Linked List", command=singly_linked_list_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
tk.Button(root, text="Doubly Linked List", command=doubly_linked_list_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
tk.Button(root, text="Queue", command=queue_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
tk.Button(root, text="Priority Queue", command=priority_queue_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
tk.Button(root, text="Binary Tree", command=binary_tree_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
tk.Button(root, text="Graph", command=graph_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
tk.Button(root, text="Traveling Salesman Problem", command=tsp_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)
tk.Button(root, text="Hash Table", command=hash_table_operations, font=LARGE_FONT, bg=BUTTON_COLOR, height=2, width=20).pack(pady=5)

root.mainloop()
