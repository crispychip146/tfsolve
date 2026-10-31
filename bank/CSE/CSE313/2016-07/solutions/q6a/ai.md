---
author: ai
via: claude-code
model: claude-sonnet-5-5
prompt: solve-v1
created: 2026-10-06
status: unverified
summary: "FAT cluster chains, FFS cylinder-group locality, NTFS runlists and resident attributes; exact allocation heuristics depend on implementation."
sources: ["Anderson and Dahlin, OSPP, ch. 13 (locality heuristics of FAT, FFS and NTFS)"]
changes:
  - "2026-10-31: Replaced universal allocator claims with the actual locality mechanisms and their limits."
---
All three systems must map logical file offsets to disk locations, but their metadata and placement mechanisms give different opportunities for locality.

| System | Mapping and locality | Main limitation |
|:--|:--|:--|
| FAT | A cluster chain, recorded in a central allocation table; choosing nearby free clusters can keep a growing file contiguous | Fragmented chains require traversal; unlike FFS, there is no inode/data cylinder-group policy |
| FFS | Put a file's inode near its directory and its data near its inode, using cylinder groups; spread directories and large-file chunks to preserve free space | Locality is a heuristic, and a large file deliberately spans groups |
| NTFS | Store small attribute values in the MFT record and larger file data in runs of contiguous clusters, described by runlists | Fragmentation creates more runs and retrieval metadata; MFT growth also needs reserved space |

FAT is not inherently forbidden from allocating contiguous clusters: sequential or remembered-position allocation can create locality while free space permits it. Also, a cached FAT avoids a disk seek to the FAT for every chain step.

FFS tracks free blocks/inodes per group and traditionally reserves some free space so nearby placement remains possible as the volume fills. The reservation percentage is a configuration/design choice, not a universal law.

NTFS uses a free-cluster bitmap and run-based allocation, with space reserved for MFT growth. Descriptions such as "always best-fit" or "the MFT is always at the disk's middle" are not general guarantees. The comparison that matters is **cluster chains**, **group-based placement**, and **run-based allocation with resident metadata/data**.
