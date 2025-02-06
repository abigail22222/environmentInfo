When you download and install Python from [python.org](https://www.python.org/), the standard installation includes the following key components:

### **1. Python Interpreter**

- This is the core component that runs Python code.
- When you execute `python script.py`, the interpreter reads and executes the script.

### **2. Standard Library (stdlib)**

- A collection of built-in modules that provide essential functionality.
- Examples include:
  - `os` (operating system interaction)
  - `sys` (system-specific parameters and functions)
  - `math` (mathematical functions)
  - `json` (JSON parsing)
  - `random` (random number generation)
  - `datetime` (date and time manipulation)
  - `re` (regular expressions)
  - Many more...

### **3. `pip` (Package Manager)**

- Installed by default (since Python 3.4).
- Allows installing third-party libraries from **PyPI** (`pip install package_name`).
- Located in `Scripts/` (Windows) or part of the Python environment (Linux/macOS).

### **4. `IDLE` (Python's Built-in IDE)**

- A simple integrated development environment (IDE).
- Includes a Python shell for quick testing.
- Useful for beginners but not as feature-rich as VS Code, PyCharm, etc.

### **5. `venv` (Virtual Environment Manager)**

- Allows you to create isolated environments for projects.

- Prevents package conflicts between projects.

- Example usage:

  ```sh
  python -m venv myenv
  source myenv/bin/activate  # macOS/Linux
  myenv\Scripts\activate     # Windows
  ```

### **6. `ensurepip` (Ensures `pip` is Installed)**

- Allows reinstallation of 

  ```
  pip
  ```

   if needed:

  ```sh
  python -m ensurepip --default-pip
  ```

### **7. Command-Line Tools**

- `python` / `python3`: Runs Python interactively or executes scripts.
- `pip`: Manages packages.
- `venv`: Creates virtual environments.
- `idle`: Opens IDLE.
- `python -m module_name`: Runs a module as a script.

### **What’s \*Not\* Included?**

- Third-party libraries (e.g., NumPy, Pandas, Flask, Django) must be installed using `pip`.
- A full-featured IDE like VS Code or PyCharm.

Would you like help setting up Python or installing packages?