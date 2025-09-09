---
description: Learn the different types of links and how to create one.
---

# Links

## Soft Links

Soft links (symbolic links) point to existing files and can span multiple filesystems.  
They are **special files** that reference another file.

**Create a soft link**:  
`ln -s /etc/grub2.cfg grub2.cfg`

Example output from `ls -l`:  
`lrwxrwxrwx. 1 root root   14 Feb  1 05:05 grub2.cfg -> /etc/grub2.cfg`

⚠️ If the original file is deleted, the soft link becomes a **dangling link**.  
If a new file is created with the same name, the link points to that new file instead.

---

## Hard Links

Hard links share the same **inode** as the original file.  
- Permissions, ownership, timestamps, and content are identical.  
- If the original file is deleted, data still exists via the hard link.  
- Data is removed only when all hard links are deleted.  

To check if files are hard-linked, compare their inode numbers:  
`ls -i file1 file2`

**Create a hard link**:  
`ln /etc/hosts /tmp/hlhosts`

---

✅ Now that you know the difference between soft and hard links, let’s move on to **common text tools and grepping**.
