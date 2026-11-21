<div align="center">

<img src="./.github/assets/readme/oop-logo.svg" alt="Object Oriented Programming logo" width="88" />

# Object Oriented Programming

**Three CSE 108 offline assignments, in C++ and Java.**

Constructors and destructors, method overloading with dynamic objects, and
inheritance with interfaces and exceptions.

![C++](https://img.shields.io/badge/C%2B%2B-3_files-00599c?logo=cplusplus&logoColor=white)
![Java](https://img.shields.io/badge/Java-4_files-ed8b00?logo=openjdk&logoColor=white)
![Course](https://img.shields.io/badge/BUET-CSE%20108-555555)

[Assignments](#assignments) ·
[Design](#design) ·
[Run them](#run-them) ·
[Notes](#notes)

</div>

## At a glance

| | Offline 1 | Offline 2 | Offline 3 |
| --- | --- | --- | --- |
| **Language** | C++ | C++ | Java |
| **Topic** | Encapsulation, constructors, destructor, overloaded `getsum` | Dynamic objects, overloaded constructors, copy constructor, passing and returning objects | Inheritance, interface, custom checked exception |
| **Program** | A `Figure` class holding a matrix of points | An election system with `PartyMember` and `PoliticalParty` | A car workshop with `Car`, `Bike` and `Truck` |
| **Folder** | `Constructor, Destructor And Overloading/` | `Method Overloading/` | `Java Inheritance/` |
| **Deadline** | 12 December 2025 | 9 January 2026 | 6 February 2026 |

Each folder also holds the assignment specification as a PDF or Word file.

## Assignments

### Offline 1: the `Figure` class

A line, a triangle or a rectangle is stored as a matrix: one row per point, one column per dimension. `Figure` owns that matrix as a heap-allocated `int**`, keeps `row`, `col`, `name` and `matrix` private, and names itself from the shape of the matrix.

| Rows x columns | Name |
| --- | --- |
| 2 x 2 / 2 x 3 | Line 2D / Line 3D |
| 3 x 2 / 3 x 3 | Triangle 2D / Triangle 3D |
| 4 x 2 / 4 x 3 | Rectangle 2D / Rectangle 3D |
| anything else | Unknown Figure |

- **Three constructors:** default (empty), parameterised `(r, c, int* data)` which unpacks a 1D array into the matrix, and a clone constructor.
- **Destructor:** prints `<name> has been destroyed`, zeroes every element, frees the rows and the array.
- **Overloaded `getsum`:** `getsum()` adds everything; `getsum(x, y)` adds rows `0..x-1` and columns `0..y-1`, and prints an error and returns `-1` when `x` or `y` is not smaller than the matrix size.
- **Code reuse:** one private `reuse(r, c)` allocates, one private `reuse_clean()` frees, and every constructor and the destructor go through them.

The real output of `main()`, which builds a 4 x 3 rectangle, clones it and runs four sum tests:

<img src="./.github/assets/readme/oop-figure-run.jpg" alt="Terminal output of the Figure program showing construction, the clone, four sum tests and three destructor messages" width="100%" />

### Offline 2: the election system

`PartyMember` holds an id, a name, an asset value and the constituency the member is nominated for. `PoliticalParty` keeps up to 1,000 members in an array of `PartyMember*`, allocating an object only when someone joins and freeing it when they leave. No STL containers are used, as the specification requires.

| Operation | What it does |
| --- | --- |
| `join_party(PartyMember)` | Copies the member in and gives them the next id. Ids are never reused. |
| `join_party(PoliticalParty&)` | Moves every member of the other party over, clears their nominations and empties the other party. |
| `leave_party(id)` | Deletes the member and closes the gap in the array. |
| `nominate_member(id, area)` / `cancel_nomination(id)` | Sets or clears the constituency. |
| `search_member(area)` | Returns the nominated member by value, or an empty `PartyMember`. |
| `form_new_party(name)` | Moves every member who has no nomination into a new party. |
| `show_all_members()` / `show_nominated_members()` | Print in the format of the reference output. |

The output of the supplied `main()`. It matches the expected-output comment in the file line for line, except that the reference leaves a blank line between blocks and the program does not print one:

<img src="./.github/assets/readme/oop-election-run.jpg" alt="Terminal output of the election program: members of ABC, nominations, a merge of XYZ into ABC and the new party Renegades" width="100%" />

### Offline 3: the car workshop

A console program, "MECHANIK", that reads vehicles from the keyboard. `Vehicle` is abstract, `Car`, `Bike` and `Truck` extend it, and each also implements the `Servicable` interface.

| Vehicle | Extra field | Service cost |
| --- | --- | --- |
| Car | number of doors | base + doors x 500 |
| Bike | engine capacity | base + capacity x 2 |
| Truck | load capacity | base + capacity x 1000 |

`ServiceException` is a custom checked exception. It is thrown for an empty or null registration number, for a negative base cost (both messages at once if both are wrong), and from `service()` when the cost goes above `MAX_LIMIT`, 500,000,000. `Main` handles it with `try`, `catch` and `finally`, and a vehicle is only added to the workshop after it has been serviced successfully. At the end the program lists every vehicle and prints the total revenue.

A real session: three valid vehicles, a car with an empty registration number and a negative cost, a car whose cost is over the limit, then `Exit`:

<img src="./.github/assets/readme/oop-workshop-run.jpg" alt="Terminal output of the workshop program: three vehicles added, two rejected with ServiceException messages, then the vehicle list and total revenue of 47300.0 BDT" width="100%" />

## Design

### `Figure` (Offline 1)

```mermaid
classDiagram
    class Figure {
        -int row
        -int col
        -string name
        -int** matrix
        -reuse(r, c)
        -reuse_clean()
        +Figure()
        +Figure(r, c, data)
        +Figure(Figure copy)
        +getsum() int
        +getsum(x, y) int
        +~Figure()
    }
```

### Election system (Offline 2)

```mermaid
classDiagram
    class PartyMember {
        -int member_id
        -string name
        -long asset_value
        -string nominated_for
        +show_member_details()
    }
    class PoliticalParty {
        -string name
        -PartyMember* members[1000]
        -int member_count
        -int current_id
        +join_party(PartyMember)
        +join_party(PoliticalParty)
        +leave_party(id)
        +nominate_member(id, area)
        +cancel_nomination(id)
        +search_member(area) PartyMember
        +form_new_party(name) PoliticalParty
    }
    PoliticalParty "1" o-- "0..1000" PartyMember : owns
```
