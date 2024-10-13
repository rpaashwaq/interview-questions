
What is difference between soft link and hard link

## Comparison of Soft Links and Hard Links

| Feature | Soft Link | Hard Link |
|---|---|---|
| **Nature** | Pointer to the original file. | Additional names for the same file. |
| **Creation Command** | `ln -s` | `ln` |
| **Cross-Filesystem Capability** | Can point to files on different file systems. | Cannot point to files on different file systems. |
| **Impact of Original File Deletion** | Becomes useless. | Remains functional. |
| **Inode Numbers** | Different inode number. | Same inode number as the original file. |
