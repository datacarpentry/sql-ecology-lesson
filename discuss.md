---
title: Discussion
---

## Proposed Episode on Database Design

### Q \& A on Database Design (review if time)

1. Order doesn't matter
2. Every row-column combination contains a single *atomic* value, i.e., not
  containing parts we might want to work with separately.
3. One field per type of information
4. No redundant information
  - Split into separate tables with one table per class of information
  - Needs an identifier in common between tables – shared column - to
    reconnect (foreign key).

### Naming

- Naming conventions are important because:
  \* Names are used more than once
  \* Names are not usually subject to change (for example, spelling errors)

- Names should uniquely identify what the data is representing

- Names should be descriptive and familiar

- Names should adhere to certain standards: (General Tips)
  \* Names should not contain spaces (for example, nameofdata)
  \* Names should not start with numbers, rather add numbers at the end of the name (for example, name$
  \* Names should be full words, not abbreviations (for example, doctor)
  \* Underscores can separate words (for example, name\_of\_data)
  \* Data types are not names (for example, integer\_data instead of integer)
  \* Names are often in lowercase (for example, name)
  \* DO NOT use quotes when naming a table or field

### Other database management systems

- Access or Filemaker Pro
  
  - GUI
  - Forms w/QAQC
  - But not cross-platform

- MySQL/PostgreSQL
  
  - Multiple simultaneous users
  - More difficult to setup and maintain

- Microsoft SQL Server (SQL Server)
  
  - Multiple simultaneous users
  - Easy to use
  - Most complex part is development and troubleshooting

- MongoDB (for Big Data needs)
  
  - Easy to scale
  - Performance is good since it is equipped to work with big data


