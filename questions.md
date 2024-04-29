# Mock Interview Questions from 4/29/2024 at MSU Denver

## Question 1 - Pathfinding/Searching Problem 

This question tests a student's understanding of algorithms and traversing through 2d matrix. 

### Interview Question:
**Design a program for a Roomba-style vacuum that ensures it cleans each cell of a square room exactly once. Assume the room is represented as a grid of equal-sized cells. The Roomba can move up, down, left, or right into adjacent cells. Provide an algorithm that guarantees every cell is visited once without revisiting any cell.**

### Points to Consider:
1. **Room Representation:** The room is represented as a 2D grid (matrix) of cells.
2. **Movement Constraints:** The Roomba can move in four directions: up, down, left, or right.
3. **Starting Point:** You may assume the Roomba starts at any cell you choose, typically a corner or the center.
4. **Algorithm Design:** Focus on designing an algorithm that efficiently covers all cells without repetition.

### Potential Solution Approach: Depth-First Search (DFS)
You can suggest using Depth-First Search (DFS) as one potential solution. DFS is a common algorithm for exploring all nodes in a graph, which can be adapted to ensure that each cell in the grid is visited exactly once. Here’s a broad overview of how this might be implemented:

1. **Initialize:** Start at a corner of the grid (e.g., top-left corner at coordinates (0, 0)).
2. **Recursive Exploration:** Use a recursive function to explore each adjacent cell. Before moving into a cell, check if it has already been visited.
3. **Marking Visits:** Keep a 2D array (or use a set of tuples) to mark visited cells, ensuring that the Roomba does not revisit any cell.
4. **Boundary Checks:** When attempting to move to an adjacent cell, ensure the move doesn't go outside the grid boundaries.

### Conceptual example in Python:
```python
def clean_room(grid, x, y, visited):
    # Directions the Roomba can move
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]  # Right, Down, Left, Up
    
    # Mark the current cell as visited
    visited.add((x, y))
    
    # Try all possible movements
    for dx, dy in directions:
        nx, ny = x + dx, y + dy
        # Check if the new cell is within bounds and not visited
        if 0 <= nx < len(grid) and 0 <= ny < len(grid[0]) and (nx, ny) not in visited:
            clean_room(grid, nx, ny, visited)

def start_cleaning(grid):
    visited = set()
    # Assuming Roomba starts at top-left corner
    clean_room(grid, 0, 0, visited)

# Example grid, where True indicates the cell is cleanable
grid = [[True]*n for _ in range(n)]
start_cleaning(grid)
```

## Question 2 - Two Sum Problem 
This problem tests knowledge of hash tables and problem solving.
### Interview Question:
**Design a function that takes an array of integers and returns the indices of the two numbers that add up to a specific target. You may assume that each input would have exactly one solution, and you may not use the same element twice.**

### Sample Solution Approach 

This solution uses a  hash table (dictionary in Python) to store the numbers and their indices as we iterate through the array. This way, we can check if the complement (target - current number) exists in our table in constant time.

#### Solution code in Python:

```python
def two_sum(nums, target):
    # Dictionary to hold number and its index
    num_to_index = {}
    
    # Iterate through the list of numbers
    for index, num in enumerate(nums):
        # Calculate complement
        complement = target - num
        
        # Check if the complement exists in the dictionary
        if complement in num_to_index:
            return [num_to_index[complement], index]
        
        # If not found, add the number and its index to the dictionary
        num_to_index[num] = index

    # Return an empty list if no solution is found
    return []

# Example usage:
nums = [2, 7, 11, 15]
target = 9
result = two_sum(nums, target)
print("Indices:", result)  # Output will be: Indices: [0, 1]
```

This function works by iterating through the array once. For each element, it checks if its complement (i.e., `target - current element`) already exists in the dictionary. If it exists, it means we have found the two numbers whose sum is equal to the target, and we return their indices. If it does not exist, we add the current number and its index to the dictionary. This approach runs in O(n) time complexity and O(n) space complexity, making it efficient for this problem.

## Question 3 - Class Design and OOP 


This question tests a students knowledge of foundational OOP concepts and class Design. 

### Interview Question:
**Design a class system for a library management system. The system should be able to handle books, patrons, and transactions such as checkouts and returns. Explain how you would use the principles of encapsulation, inheritance, and polymorphism to structure your classes and their relationships.**

### Points to Consider:
1. **Class Design:** Ask the candidate to design key classes like `Book`, `Patron`, `Library`, and `Transaction`.
2. **Attributes and Methods:** Each class should have relevant attributes (e.g., `Book` might have `title`, `author`, `ISBN`) and methods (e.g., `check_out`, `return_book`).
3. **Relationships Between Classes:** Candidates should explain how these classes interact. For instance, a `Library` might have a list of `Book` objects and manage `Transaction` objects that relate books and patrons.
4. **Use of OOP Principles:**
   - **Encapsulation:** Data (attributes) and methods that manipulate the data are encapsulated as a single unit. For example, book-related data and operations can be encapsulated in the `Book` class.
   - **Inheritance:** Candidates might create a subclass from a general class. For instance, `Magazine` and `Newspaper` could inherit from a superclass `Item`, with `Book` being another subclass.
   - **Polymorphism:** The use of polymorphism to handle different types of transactions, like renewals versus regular checkouts, which might have different rules or processes.
5. **Error Handling:** How to handle common library scenarios such as overdue books or unavailable books.

### Example Solution Structure in Python:
Here is a basic outline that a candidate might propose, using Python to illustrate the concept:

```python
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_checked_out = False

    def check_out(self, patron):
        if not self.is_checked_out:
            self.is_checked_out = True
            print(f"{self.title} has been checked out to {patron.name}.")
        else:
            print(f"{self.title} is currently unavailable.")

    def return_book(self):
        self.is_checked_out = False
        print(f"{self.title} has been returned.")

class Patron:
    def __init__(self, name, patron_id):
        self.name = name
        self.patron_id = patron_id
        self.books_checked_out = []

    def check_out_book(self, book):
        book.check_out(self)
        self.books_checked_out.append(book)

    def return_book(self, book):
        book.return_book()
        self.books_checked_out.remove(book)

class Library:
    def __init__(self):
        self.books = []
        self.patrons = []

    def add_book(self, book):
        self.books.append(book)

    def register_patron(self, patron):
        self.patrons.append(patron)

# Example Usage
library = Library()
book1 = Book("1984", "George Orwell", "123456789")
patron1 = Patron("John Doe", "001")
library.add_book(book1)
library.register_patron(patron1)
patron1.check_out_book(book1)
patron1.return_book(book1)
```

This question allows the candidate to demonstrate their understanding of OOP concepts by designing a flexible and extensible system, and how well they can articulate their reasoning and the design choices they make.