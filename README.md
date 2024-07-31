# GoIT Homework: Relational Database Management

## 1. Database Creation

### Database Schema: "LibraryManagement"

#### a. Create Table `authors`

```sql
CREATE TABLE authors (
    author_id INT AUTO_INCREMENT PRIMARY KEY,
    author_name VARCHAR(255) NOT NULL
);
