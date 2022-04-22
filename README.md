# robotoc_vendor
This is a CMake wrapper around [robotoc](https://github.com/mayataka/robotoc), a whole-body MPC solver.

# Requirements
- [Eigen3](http://eigen.tuxfamily.org/index.php?title=Main_Page)
- [Pinocchio](https://github.com/stack-of-tasks/pinocchio)

# Usage
```
find_package(robotoc_vendor REQUIRED)

ament_target_dependencies(my_target robotoc_vendor)
```
