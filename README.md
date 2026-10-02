# 📸 Instagram Clone — SQL Database Design & Analytics

> A complete **MySQL relational database project** that models the core data structure of an Instagram-style social media platform and solves practical SQL analytics challenges.

![SQL](https://img.shields.io/badge/SQL-MySQL-blue)
![Database](https://img.shields.io/badge/Database-Relational-green)
![Project](https://img.shields.io/badge/Project-Instagram%20Clone-purple)
![Status](https://img.shields.io/badge/Status-Completed-success)

---

## 📌 Project Overview

The **Instagram Clone SQL Project** is a database-design and SQL-analysis project built to simulate the backend data model of a social media platform.

The project starts with a relational database called `ig_clone`, creates the required tables and relationships, loads sample data, and then applies SQL queries to answer real-world analytical questions.

The project covers the complete workflow:

**Database Creation → Schema Design → Relationships → Data Loading → SQL Analysis → Business Questions → Advanced SQL Challenges**

The database models:

- Users
- Photos
- Comments
- Likes
- Followers / Following relationships
- Hashtags
- Photo–hashtag relationships

The second SQL file contains a set of business-style challenges that use the database to analyze user activity, content engagement, hashtag usage, and unusual interaction patterns.

---

# 📂 Project Files

```text
Instagram-Clone/
│
├── 14.Instagram Clone Database.sql
├── 15.Instagram Clone Database - Challenges.sql
├── README.md
│
└── images/
    └── instagram-clone-er-diagram.png
```

### File 1 — `14.Instagram Clone Database.sql`

Contains:

- Database creation
- Table creation
- Primary keys
- Foreign keys
- Composite primary keys
- Sample data insertion
- Tags
- Photo–tag mappings

### File 2 — `15.Instagram Clone Database - Challenges.sql`

Contains analytical SQL questions and solutions covering:

- Oldest users
- Registration-day analysis
- Users without photos
- Most-liked photo
- Average posts per user
- User post ranking
- Total posts
- Active posting users
- Popular hashtags
- Users who liked every photo
- Users who never commented
- User behavior percentages
- Bot / celebrity-style activity analysis

---

# 🎯 Project Objectives

The main objectives of this project are:

1. Build a normalized relational database for an Instagram-like platform.
2. Establish relationships between users, photos, comments, likes, follows and tags.
3. Maintain data integrity using primary and foreign keys.
4. Use composite keys for interaction tables.
5. Use a junction table for many-to-many relationships.
6. Insert and work with realistic sample data.
7. Solve business-oriented SQL problems.
8. Practice joins, aggregation, subqueries and filtering.
9. Analyze user engagement and content performance.
10. Demonstrate practical SQL skills through an end-to-end project.

---

# 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| **MySQL** | Database management |
| **SQL** | Database creation and analysis |
| **ER Modeling** | Database relationship design |
| **GitHub** | Project version control and portfolio |
| **Markdown** | Project documentation |

---

# 🗃️ Database Name

```sql
CREATE DATABASE ig_clone;

USE ig_clone;
```

The complete project works inside the `ig_clone` database.

---

# 🏗️ Database Architecture

The database contains **7 core tables**:

```text
users
   │
   ├─────────────── photos
   │                   │
   │                   ├──────── comments
   │                   │
   │                   └──────── likes
   │
   ├─────────────── follows
   │
   └─────────────── comments / likes

photos ───────── photo_tags ───────── tags
```

### High-level relationship model

```text
Users
  │
  ├── 1 : N ── Photos
  │              │
  │              ├── 1 : N ── Comments
  │              │
  │              ├── 1 : N ── Likes
  │              │
  │              └── N : M ── Tags
  │                            │
  │                       Photo_Tags
  │
  └── N : M ── Users
              via Follows
```

---

# 🖼️ ER Diagram

![Instagram Clone ER Diagram](images/instagram-clone-er-diagram.png)

The ER diagram represents the database entities, attributes, primary keys, foreign keys and relationships.

---

# 📊 Database Tables

## 1. `users`

Stores information about platform users.

| Column | Data Type | Description |
|---|---|---|
| `id` | INT | Primary key |
| `username` | VARCHAR(255) | Unique-style user identifier in the sample data |
| `created_at` | TIMESTAMP | User registration timestamp |

### Key

```text
PK → id
```

---

## 2. `photos`

Stores photos uploaded by users.

| Column | Data Type | Description |
|---|---|---|
| `id` | INT | Primary key |
| `image_url` | VARCHAR(355) | URL of the image |
| `user_id` | INT | User who uploaded the photo |
| `created_dat` | TIMESTAMP | Photo creation timestamp |

### Relationship

```text
users.id → photos.user_id
```

A single user can upload multiple photos.

> **Schema note:** The original project uses the column name `created_dat` (without the final `e`). The README keeps the original schema name so the documentation matches the supplied SQL.

---

## 3. `comments`

Stores comments made by users on photos.

| Column | Data Type | Description |
|---|---|---|
| `id` | INT | Primary key |
| `comment_text` | VARCHAR(255) | Comment content |
| `user_id` | INT | User who posted the comment |
| `photo_id` | INT | Photo being commented on |
| `created_at` | TIMESTAMP | Comment timestamp |

### Relationships

```text
users.id  → comments.user_id
photos.id → comments.photo_id
```

---

## 4. `likes`

Stores photo likes.

| Column | Data Type | Description |
|---|---|---|
| `user_id` | INT | User who liked the photo |
| `photo_id` | INT | Photo that was liked |
| `created_at` | TIMESTAMP | Like timestamp |

The table uses a **composite primary key**:

```sql
PRIMARY KEY(user_id, photo_id)
```

This prevents the same user from creating duplicate likes for the same photo.

---

## 5. `follows`

Stores follower/following relationships between users.

| Column | Data Type | Description |
|---|---|---|
| `follower_id` | INT | User who follows |
| `followee_id` | INT | User being followed |
| `created_at` | TIMESTAMP | Relationship creation time |

The table uses:

```sql
PRIMARY KEY(follower_id, followee_id)
```

This models a **many-to-many self-referencing relationship** on the `users` table.

---

## 6. `tags`

Stores hashtags.

| Column | Data Type | Description |
|---|---|---|
| `id` | INT | Primary key |
| `tag_name` | VARCHAR(255) | Hashtag name |
| `created_at` | TIMESTAMP | Tag creation timestamp |

The project applies:

```sql
UNIQUE(tag_name)
```

so the same tag name cannot be inserted repeatedly.

Example tags include:

```text
sunset
photography
sunrise
landscape
food
foodie
delicious
beauty
fashion
party
beach
smile
```

---

## 7. `photo_tags`

This is the **junction / bridge table** between photos and tags.

| Column | Data Type | Description |
|---|---|---|
| `photo_id` | INT | Related photo |
| `tag_id` | INT | Related tag |

Composite primary key:

```sql
PRIMARY KEY(photo_id, tag_id)
```

### Relationship

```text
photos N : M tags
```

A photo can have multiple tags, and a tag can be used on multiple photos.

---

# 🔑 Key Database Concepts

## Primary Keys

Primary keys uniquely identify records.

Examples:

```sql
users.id
photos.id
comments.id
tags.id
```

Composite primary keys are used in:

```sql
likes(user_id, photo_id)

follows(follower_id, followee_id)

photo_tags(photo_id, tag_id)
```

---

## Foreign Keys

Foreign keys maintain relationships between tables.

Examples:

```sql
FOREIGN KEY(user_id) REFERENCES users(id)

FOREIGN KEY(photo_id) REFERENCES photos(id)
```

This helps maintain **referential integrity**.

---

# 🔗 Relationship Types

### Users → Photos

```text
1 : N
```

One user can upload many photos.

### Photos → Comments

```text
1 : N
```

One photo can receive many comments.

### Photos → Likes

```text
1 : N
```

One photo can receive many likes.

### Users → Follows → Users

```text
N : M
```

A user can follow many users, and a user can be followed by many users.

### Photos → Tags

```text
N : M
```

Implemented through:

```text
photo_tags
```

---

# 📥 Data Loading

The main SQL file contains sample records for the database.

The project includes data for:

- Users
- Photos
- Comments
- Likes
- Follows
- Tags
- Photo–tag mappings

The supplied dataset contains approximately:

| Table | Records |
|---|---:|
| Users | 100 |
| Photos | 257 |
| Comments | 7,488 |
| Likes | 8,782 |
| Follows | 7,623 |
| Tags | 21 |
| Photo Tags | 502 |

These counts are based on the supplied `INSERT` statements.

---

# 🚀 How to Run the Project

## Step 1 — Install MySQL

Install one of:

- MySQL Server
- MySQL Workbench
- XAMPP with MySQL
- Another MySQL-compatible environment

---

## Step 2 — Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/instagram-clone-sql.git
```

Move into the project:

```bash
cd instagram-clone-sql
```

---

## Step 3 — Open MySQL

Open MySQL Workbench or your preferred SQL client.

---

## Step 4 — Run the Database SQL File

Open:

```text
14.Instagram Clone Database.sql
```

Run the complete script.

It will:

1. Create `ig_clone`
2. Select the database
3. Create all tables
4. Create relationships
5. Insert sample data

---

## Step 5 — Verify the Database

```sql
SHOW DATABASES;

USE ig_clone;

SHOW TABLES;
```

Expected tables:

```text
users
photos
comments
likes
follows
tags
photo_tags
```

---

## Step 6 — Check the Data

```sql
SELECT COUNT(*) FROM users;

SELECT COUNT(*) FROM photos;

SELECT COUNT(*) FROM comments;

SELECT COUNT(*) FROM likes;

SELECT COUNT(*) FROM follows;

SELECT COUNT(*) FROM tags;

SELECT COUNT(*) FROM photo_tags;
```

---

# 🧠 SQL Analysis & Business Challenges

The challenge file is designed around real business questions.

---

## 1. Find the 5 Oldest Users

### Business Question

> Which users have been on the platform the longest?

```sql
SELECT *
FROM users
ORDER BY created_at
LIMIT 5;
```

### Concepts

- `ORDER BY`
- Ascending date sorting
- `LIMIT`

---

# 2. Find the Most Popular Registration Day

### Business Question

> Which day of the week has the highest number of user registrations?

```sql
SELECT
    DAYNAME(created_at) AS day,
    COUNT(*) AS total
FROM users
GROUP BY day
ORDER BY total DESC;
```

### Concepts

- `DAYNAME()`
- `GROUP BY`
- `COUNT()`
- `ORDER BY`

---

# 3. Find Users Who Never Posted a Photo

### Business Question

> Which users have registered but never uploaded content?

```sql
SELECT username
FROM users
LEFT JOIN photos
    ON users.id = photos.user_id
WHERE photos.id IS NULL;
```

### Key Concept

This demonstrates an **anti-join pattern** using:

```text
LEFT JOIN + IS NULL
```

---

# 4. Find the Photo With the Most Likes

### Business Question

> Which photo received the highest number of likes?

```sql
SELECT
    users.username,
    photos.id,
    photos.image_url,
    COUNT(*) AS total_likes
FROM likes
JOIN photos
    ON photos.id = likes.photo_id
JOIN users
    ON users.id = photos.user_id
GROUP BY photos.id
ORDER BY total_likes DESC
LIMIT 1;
```

### Concepts

- Multiple table joins
- Aggregation
- `COUNT()`
- `GROUP BY`
- `ORDER BY`
- `LIMIT`

---

# 5. Calculate Average Posts Per User

### Business Question

> How many photos does the average user post?

```sql
SELECT ROUND(
    (SELECT COUNT(*) FROM photos) /
    (SELECT COUNT(*) FROM users),
    2
);
```

### Concepts

- Scalar subqueries
- Arithmetic operations
- `ROUND()`

---

# 6. Rank Users by Number of Posts

```sql
SELECT
    users.username,
    COUNT(photos.image_url) AS total_posts
FROM users
JOIN photos
    ON users.id = photos.user_id
GROUP BY users.id
ORDER BY total_posts DESC;
```

This identifies the most active content creators based on post volume.

---

# 7. Find Total Number of Posts

The project also demonstrates using a derived table:

```sql
SELECT SUM(user_posts.total_posts_per_user)
FROM (
    SELECT
        users.username,
        COUNT(photos.image_url) AS total_posts_per_user
    FROM users
    JOIN photos
        ON users.id = photos.user_id
    GROUP BY users.id
) AS user_posts;
```

### Concepts

- Derived tables
- Nested queries
- `SUM()`
- `COUNT()`

---

# 8. Count Users Who Have Posted At Least Once

```sql
SELECT COUNT(DISTINCT users.id) AS total_number_of_users_with_posts
FROM users
JOIN photos
    ON users.id = photos.user_id;
```

### Concepts

- `COUNT(DISTINCT ...)`
- `JOIN`

---

# 9. Find Popular Hashtags

### Business Question

> Which hashtags are used most frequently?

```sql
SELECT
    tag_name,
    COUNT(tag_name) AS total
FROM tags
JOIN photo_tags
    ON tags.id = photo_tags.tag_id
GROUP BY tags.id
ORDER BY total DESC;
```

### Concepts

- Junction-table joins
- Many-to-many relationships
- Aggregation

> **Note:** The original challenge wording asks for the top 5 hashtags, while the supplied query orders all tags but does not include `LIMIT 5`. To return exactly five, use `LIMIT 5`.

---

# 10. Find Users Who Liked Every Photo

### Business Question

> Which users have liked every photo in the database?

```sql
SELECT
    users.id,
    username,
    COUNT(users.id) AS total_likes_by_user
FROM users
JOIN likes
    ON users.id = likes.user_id
GROUP BY users.id
HAVING total_likes_by_user = (
    SELECT COUNT(*)
    FROM photos
);
```

### Concepts

- `GROUP BY`
- `HAVING`
- Scalar subquery
- Aggregate comparison

---

# 11. Find Users Who Never Commented

```sql
SELECT
    username,
    comment_text
FROM users
LEFT JOIN comments
    ON users.id = comments.user_id
GROUP BY users.id
HAVING comment_text IS NULL;
```

This uses a `LEFT JOIN` to retain users even when no matching comment exists.

---

# 12. Count Users Who Never Commented

```sql
SELECT COUNT(*)
FROM (
    SELECT
        username,
        comment_text
    FROM users
    LEFT JOIN comments
        ON users.id = comments.user_id
    GROUP BY users.id
    HAVING comment_text IS NULL
) AS total_number_of_users_without_comments;
```

### Concepts

- Subquery
- Derived table
- `COUNT()`

---

# 13. Find Users Who Have Commented

```sql
SELECT
    username,
    comment_text
FROM users
LEFT JOIN comments
    ON users.id = comments.user_id
GROUP BY users.id
HAVING comment_text IS NOT NULL;
```

This is the complementary analysis to users with no comments.

---

# 14. User Activity Percentage Analysis

The project contains a larger challenge that compares:

- Users who never commented
- Users who liked every photo

The analysis calculates both the number of users and their percentage of the total user base.

The general calculation is:

```text
percentage =
(number of users in category / total users) × 100
```

This demonstrates combining:

- Nested subqueries
- Aggregation
- `JOIN`
- Percentage calculations
- `HAVING`

---

# 🧩 SQL Concepts Demonstrated

This project covers a broad range of practical SQL concepts.

### DDL

```sql
CREATE DATABASE
CREATE TABLE
```

### Constraints

```sql
PRIMARY KEY
FOREIGN KEY
UNIQUE
NOT NULL
```

### DML

```sql
INSERT INTO
```

### Filtering

```sql
WHERE
HAVING
```

### Aggregation

```sql
COUNT()
SUM()
ROUND()
```

### Grouping

```sql
GROUP BY
```

### Sorting

```sql
ORDER BY
```

### Result Limiting

```sql
LIMIT
```

### Joins

```sql
INNER JOIN
JOIN
LEFT JOIN
```

### Subqueries

```sql
SELECT (...) 
```

### Derived Tables

```sql
FROM (
    SELECT ...
) AS table_name
```

### Date Functions

```sql
DAYNAME()
DATE_FORMAT()
```

### Distinct Analysis

```sql
COUNT(DISTINCT ...)
```

---

# 🔍 Important SQL Patterns

## Anti-Join Pattern

Used to find users without photos or comments:

```sql
SELECT u.username
FROM users u
LEFT JOIN photos p
    ON u.id = p.user_id
WHERE p.id IS NULL;
```

---

## Aggregation Pattern

Used for engagement analysis:

```sql
SELECT photo_id, COUNT(*) AS total_likes
FROM likes
GROUP BY photo_id;
```

---

## Top-N Pattern

```sql
SELECT ...
FROM ...
ORDER BY metric DESC
LIMIT 5;
```

---

## Many-to-Many Pattern

```text
photos
   ↓
photo_tags
   ↓
tags
```

---

## Self-Referencing Relationship

```text
users
  ↕
follows
  ↕
users
```

The `follows` table references the `users` table twice:

```sql
FOREIGN KEY (follower_id) REFERENCES users(id)

FOREIGN KEY (followee_id) REFERENCES users(id)
```

---

# 📈 Business Insights This Database Can Support

Although this is primarily a SQL/database project, the schema can support many analytics use cases.

### User Analytics

- New registrations
- Inactive users
- Active content creators
- User posting frequency

### Content Analytics

- Most-liked photos
- Most-commented photos
- Posts per user
- Content creation patterns

### Engagement Analytics

- Likes per photo
- Comments per user
- Highly engaged users
- Users with limited engagement

### Hashtag Analytics

- Most-used hashtags
- Hashtag adoption
- Photos associated with each tag

### Social Graph Analytics

- Followers per user
- Following behavior
- Highly connected users
- User-to-user relationships

---

# 🧪 Example Validation Queries

After importing the database, run:

```sql
USE ig_clone;

SELECT * FROM users LIMIT 10;

SELECT * FROM photos LIMIT 10;

SELECT * FROM comments LIMIT 10;

SELECT * FROM likes LIMIT 10;

SELECT * FROM follows LIMIT 10;

SELECT * FROM tags;

SELECT * FROM photo_tags LIMIT 10;
```

Check table relationships:

```sql
DESCRIBE users;
DESCRIBE photos;
DESCRIBE comments;
DESCRIBE likes;
DESCRIBE follows;
DESCRIBE tags;
DESCRIBE photo_tags;
```

---

# ⚙️ Potential Improvements

The supplied project is intentionally focused on SQL learning and relational analysis. If extending it into a production-style system, possible improvements include:

### Database Design

- Add indexes to frequently filtered foreign-key columns.
- Add explicit `UNIQUE` constraints where business rules require uniqueness.
- Consider `ON DELETE` / `ON UPDATE` actions based on application requirements.
- Standardize timestamp column naming.
- Consider separating authentication data from public user profile data.

### Data Quality

- Validate URL formats at the application layer.
- Add stronger username constraints if required.
- Add checks for invalid self-follow relationships if the business rule disallows them.

### Analytics

- Add views for recurring metrics.
- Add CTE-based versions of complex analytical queries.
- Add window functions for ranking.
- Create engagement-rate metrics.
- Add time-series analysis.

---

# 📚 Learning Outcomes

By completing this project, the following skills are practiced:

- Relational database design
- ER diagram interpretation
- Primary and foreign key design
- Composite primary keys
- Many-to-many relationships
- Self-referencing relationships
- SQL joins
- Aggregate functions
- Grouping and filtering
- Subqueries
- Derived tables
- Date functions
- Business-oriented SQL analysis
- Data integrity
- Query problem solving

---

# 🏆 Project Highlights

```text
✓ 7 relational tables
✓ Primary & foreign keys
✓ Composite keys
✓ Many-to-many relationship
✓ Self-referencing user relationship
✓ Sample dataset
✓ User analytics
✓ Content analytics
✓ Engagement analytics
✓ Hashtag analytics
✓ Advanced SQL challenges
✓ Business-question driven queries
```

---

# 📁 Recommended GitHub Repository Structure

For a clean portfolio repository, use:

```text
instagram-clone-sql/
│
├── README.md
│
├── sql/
│   ├── 14.Instagram Clone Database.sql
│   └── 15.Instagram Clone Database - Challenges.sql
│
├── images/
│   └── instagram-clone-er-diagram.png
│
└── presentation/
    └── Instagram_Clone_SQL_Project_Presentation.pptx
```

---

# 👨‍💻 Author

**Rajnish Singh**

Computer Science & Engineering | SQL | Data Analytics | Power BI | Excel | Python

---

# ⭐ If You Find This Project Useful

If this project helps you understand SQL database design or analytics, feel free to **star ⭐ the repository** and explore the SQL challenges.

---

## 🔖 Topics

```text
sql
mysql
database
dbms
database-design
er-diagram
relational-database
sql-project
mysql-project
data-analytics
sql-analysis
instagram-clone
sql-challenges
business-analytics
```

---
# 👨‍💻 Author

**Rajnish Kumar**

Computer Science Engineer

Data Analyst | Excel | SQL | Python | Power BI

GitHub:https://github.com/rajnish1245

LinkedIn:https://www.linkedin.com/in/rajnish-kumar-a33889230/

⭐ If you found this project helpful, don't forget to Star this repository.

> **Project takeaway:** This project demonstrates how a real-world social-media workflow can be converted into a relational database and then analyzed using practical SQL queries.
