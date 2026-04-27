**Assignment 6 — Symbolic Links (xv6)**

### Overview

In this assignment, symbolic link support was added to the xv6 file system. A new inode type `T_SYMLINK` was introduced to represent symbolic links. The `symlink()` system call was implemented to create a symbolic link inode and store the target path string inside its data blocks.

### Implementation

The `symlink()` system call creates a new inode using the existing `create()` function with type `T_SYMLINK`. The target path is written into the inode using `writei()`. This allows the file system to persist the symbolic link target across operations.

The `sys_open()` function was modified to support symbolic link resolution. When a symbolic link is opened, the system reads the stored target path using `readi()` and performs another lookup using `namei()`. This process supports chained symbolic links.

### Loop Prevention

In order to prevent infinite loops caused by cyclic symbolic links, I implemented a depth counter. The system follows symbolic links up to a maximum depth of 10. If this limit is exceeded, the `open()` operation fails. This ensures safe handling of cycles such as `a → b → a`.

### Testing

The implementation was tested using the provided `testsymlink` program. The test verified successful symbolic link creation, correct reading through a symbolic link, and proper detection of cyclic links. All test cases passed successfully.
