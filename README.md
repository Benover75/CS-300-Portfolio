# CS-300-Portfolio
CS300 Portfolio of Artifacts: Comprehensive collection of CS 300 deliverables, including performance benchmarks and memory analysis for core data structures, a modular C++ course‑list sorter implementation, and reflective documentation of development approach.

# CS 300 Module Eight Journal

## Project One: Data Structures Analysis

In Project One, I evaluated three core data structures—**Vector**, **Hash Table**, and a **Balanced Binary Search Tree (BST)**—to compare their run‑time performance and memory characteristics when managing a catalog of Computer Science courses.

| Operation             | Vector               | Hash Table            | BST (Balanced)        |
| --------------------- | -------------------- | --------------------- | --------------------- |
| **Load Data**         | O(n)                 | O(n)                  | O(n log n)            |
| **Print Sorted List** | O(n log n) (sort)    | O(n log n) (sort keys)| O(n) (in‑order)       |
| **Search Course**     | O(n) (linear scan)   | O(1) avg.             | O(log n) avg.         |
| **Memory Usage**      | O(n) contiguous      | O(n) + bucket overhead| O(n) + node overhead  |

*Table derived from Module Six Project One documentation* :contentReference[oaicite:0]{index=0}:contentReference[oaicite:1]{index=1}

## Project Two: Course List Sorter

The following C++ implementation (`ProjectTwo.cpp`) loads course data from a CSV file, sorts the courses alphanumerically by course number, and prints the sorted list:

```cpp
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>
#include <algorithm>

//----------------------------------------------------------------------
// Course struct holds the course number and name
//----------------------------------------------------------------------
struct Course {
    std::string number;
    std::string name;
};

//----------------------------------------------------------------------
// Trim whitespace from both ends of a string
//----------------------------------------------------------------------
std::string trim(const std::string& s) {
    const char* ws = " \t\n\r";
    auto start = s.find_first_not_of(ws);
    if (start == std::string::npos) return "";
    auto end = s.find_last_not_of(ws);
    return s.substr(start, end - start + 1);
}

//----------------------------------------------------------------------
// Load courses from CSV into vector, sort, and print
//----------------------------------------------------------------------
int main() {
    std::ifstream file("courses.csv");
    if (!file) {
        std::cerr << "Error: Cannot open courses.csv\n";
        return 1;
    }

    std::vector<Course> courses;
    std::string line;
    while (std::getline(file, line)) {
        if (line.empty()) continue;
        std::stringstream ss(line);
        Course c;
        std::getline(ss, c.number, ',');
        std::getline(ss, c.name,   ',');
        c.number = trim(c.number);
        c.name   = trim(c.name);
        courses.push_back(c);
    }

    std::sort(courses.begin(), courses.end(),
        [](auto &a, auto &b) { return a.number < b.number; }
    );

    std::cout << "Sorted Computer Science Courses:\n";
    for (auto &c : courses) {
        std::cout << c.number << " – " << c.name << "\n";
    }
    return 0;
}


Reflection:

1. What was the problem you were solving in the projects for this course?
I compared how different data structures handle loading, sorting, and searching a set of Computer Science courses (Project One) and then implemented a utility to load and alphabetize that exact course list in C++ (Project Two).

2. How did you approach the problem? Consider why data structures are important to understand.
I began by designing pseudocode for each data structure, benchmarked their theoretical Big‑O properties, and then translated that insight into a concrete C++ implementation. Understanding each structure’s performance profile ensured I chose the right tool for fast lookups versus rapid sorted output.

3. How did you overcome any roadblocks you encountered?
In Project One, I mitigated JVM warm‑up skew in my benchmarks by adding warm‑up iterations before timing. In Project Two, I handled CSV inconsistencies by writing a robust trim helper and validated file I/O with clear error messages to prevent silent failures.

4. How has your work on this project expanded your approach to designing software and developing programs?
I now routinely justify data‑structure selections with both theoretical and empirical data, focusing custom code on domain logic and leveraging battle‑tested standard libraries for core operations like sorting and hashing.

5. How has your work on this project evolved the way you write programs that are maintainable, readable, and adaptable?
I adopt modular design—separating parsing, sorting, and output—use self‑documenting variable names, add concise inline comments, and implement clear error handling. This approach enhances readability, facilitates future feature additions, and supports team collaboration.
