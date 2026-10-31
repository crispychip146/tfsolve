---
author: ai
via: chat
status: unverified
imported_from: cse313_mem_fs_solved.tex
changes:
  - "2026-10-31: Preserved hard-link and open-descriptor semantics during LFS unlink."
  - "2026-09-30 (import): split from the combined (i)-(iii) solution."
---
Removing a file in lfs is handled by simply appending new information to the log rather than erasing anything in place:

1. Look up `lfsfile.txt`'s directory entry (via the parent directory data, itself found via its inode from the imap) to get its inode number.
2. Write a new version of the **parent directory's data** block (in the log) with the entry for `lfsfile.txt` removed.
3. Update the parent directory's **inode** (also newly logged) to reflect its changed data location/size.
4. Decrement the inode's hard-link count and log the updated inode and imap entry. Only when the last hard link and the last open reference are gone can the inode be freed and its imap entry invalidated. If another hard link or an open descriptor exists, its inode and data must remain accessible.

Any subsequent access to `lfsfile.txt` fails simply because the directory lookup for that name no longer finds an entry at all -- removing a name is distinct from destroying the file. Existing open descriptors can continue accessing it; blocks become eligible for cleaning only once no live reference needs them.
