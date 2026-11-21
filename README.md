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
