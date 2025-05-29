

## Topics Covered

### 1. Variables
- Types of variables: playbook, host, inventory, registered, extra vars
- Variable precedence and override order
- Best practices and naming conventions

[Read → 01-variables.md](./01-variables.md)



### 2. Data Types
- Common types: string, list, dictionary, boolean, integer
- Using lists and dicts in playbooks
- Examples of complex data structures

[Read → 02-data_types.md](./02-data-types.md)



### 3.  Loops
- `with_items`, `loop`, and `with_dict`
- Iterating over lists and dictionaries
- Using loops with conditions

[Read → 03-loops.md](./03-Loops.md)



### 4.  Conditions
- `when` clause usage
- Combining conditions with `and`, `or`, `not`
- Registering output and using it conditionally

 [Read → 04-conditions.md](./04-conditions-when.md)

---

### 5.  Functions (Jinja2 Filters)
- Built-in filters like `default`, `length`, `lower`, `upper`, `replace`, `split`, `join`
- Custom logic using Jinja2 templating
- Real-world use cases in variables and tasks

[Read → 05-functions.md](./05-functions.md)

---

##  Notes

- Consistency in variable naming improves readability.
- Always validate your syntax before running complex playbooks.
- Loops and conditions reduce redundancy and improve logic control.



🧡 Keep Automating — One Task at a Time!
