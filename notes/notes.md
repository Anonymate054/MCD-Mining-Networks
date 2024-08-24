# Neo4j Guide: Modeling a University as a Social Network

---

### Introduction
In this guide, we will model a university as a social network using Neo4j. We will cover the essential Cypher commands such as `CREATE`, `MATCH`, `MERGE`, `DETACH DELETE`, `WHERE`, `GROUP BY`, and more.

---

### Creating the University Model

#### Create nodes for students, professors, courses, and departments


```cypher
CREATE (s1:Student {name: 'Alice', student_id: 'S001', age: 20}),
       (s2:Student {name: 'Bob', student_id: 'S002', age: 22}),
       (p1:Professor {name: 'Dr. Smith', professor_id: 'P001', age: 45}),
       (c1:Course {name: 'Data Mining', course_id: 'C001', credits: 3}),
       (d1:Department {name: 'Computer Science', dept_id: 'D001'})
```

#### Create relationships between entities
```cypher
CREATE (s1)-[:ENROLLED_IN]->(c1),
       (s2)-[:ENROLLED_IN]->(c1),
       (p1)-[:TEACHES]->(c1),
       (c1)-[:BELONGS_TO]->(d1),
       (s1)-[:FRIEND_WITH]->(s2)
```

#### Match all students and return their names
```cypher
MATCH (s:Student)
RETURN s.name
```

#### Match a specific course and return the students enrolled in it
```cypher
MATCH (s:Student)-[:ENROLLED_IN]->(c:Course {name: 'Data Mining'})
RETURN s.name
```

#### Match professors who teach a specific course
```cypher
MATCH (p:Professor)-[:TEACHES]->(c:Course {name: 'Data Mining'})
RETURN p.name
```

#### Match students who are older than 21
```cypher
MATCH (s:Student)
WHERE s.age > 21
RETURN s.name
```

#### Match students enrolled in a specific department
```cypher
MATCH (s:Student)-[:ENROLLED_IN]->(c:Course)-[:BELONGS_TO]->(d:Department {name: 'Computer Science'})
RETURN s.name
```

#### Merge a student into the graph, creating it if it doesn't exist
```cypher
MERGE (s:Student {student_id: 'S003', name: 'Charlie', age: 23})
```

#### Merge a friendship relationship, creating it if it doesn't exist
```cypher
MATCH (s1:Student {name: 'Alice'}), (s2:Student {name: 'Charlie'})
MERGE (s1)-[:FRIEND_WITH]->(s2)
```

#### Merge a friendship relationship, creating it if it doesn't exist
```cypher
MATCH (s1:Student {name: 'Alice'}), (s2:Student {name: 'Charlie'})
MERGE (s1)-[:FRIEND_WITH]->(s2)
```

#### Update a student's age
```cypher
MATCH (s:Student {name: 'Bob'})
SET s.age = 23
RETURN s
```

#### Add a new property to a course
```cypher
MATCH (c:Course {name: 'Data Mining'})
SET c.semester = 'Fall 2024'
RETURN c
```

#### Remove the 'semester' property from a course
```cypher
MATCH (c:Course {name: 'Data Mining'})
REMOVE c.semester
RETURN c
```

#### Delete a specific relationship
```cypher
MATCH (s:Student {name: 'Alice'})-[r:
```

### Example:

```cypher
CREATE (n3:student {name: "Vlad", student_id: "S002", age: 25})-[:ENROLLED_IN {semester: "2023-S1"}]->(n2:course {name: "Machine Learning 2", course_id: "C002", credits: 6})<-[:ENROLLED_IN {semester: "2024-S2"}]-(n0:student {name: "Luis", student_id: "S001", age: 20})-[:ENROLLED_IN {semester: "2024_H2"}]->(n1:course {name: "Mineria de Datos", course_id: "C001", credits: 3})<-[:ENROLLED_IN {semester: "2024-S2"}]-(n3)-[:ENROLLED_IN {semester: "2024-S2"}]->(n8:course {name: "Econometria para Ciencia de Datos", course_id: "C003", credits: 6})<-[:ENROLLED_IN {semester: "2024-S2"}]-(n0),
(n6:professor {name: "Leon", professor_id: "P002"})-[:IS_MEMBER_OF]->(n7:department {name: "Ingenieria", department_id: "D001"})<-[:BELONGS_TO]-(n2)<-[:TEACHES {semester: "2023-S1"}]-(n6)-[:TEACHES {semester: "2023-S2"}]->(n2)<-[:TEACHES {semester: "2024-S1"}]-(n6)-[:TEACHES {semester: "2024-S2"}]->(n2)<-[:ENROLLED_IN {semester: "2024-S2"}]-(n4:student {name: "Adrian", student_id: "S003", age: 25})-[:ENROLLED_IN {semester: "2024-S2"}]->(n1)<-[:TEACHES {semester: "2024-S1"}]-(n5:professor {name: "Alexandra", professor_id: "P001"})-[:TEACHES {semester: "2024-S2"}]->(n1)-[:BELONGS_TO]->(n7)<-[:IS_MEMBER_OF]-(n9:professor {name: "Julieta", professor_id: "P003"}),
(n4)-[:ENROLLED_IN {semester: "2024-S2"}]->(n8)<-[:TEACHES {semester: "2024-S2"}]-(n9)-[:IS_MEMBER_OF]->(n10:department {name: "Economia", department_id: "D002"}),
(n5)-[:IS_MEMBER_OF]->(n7),
(n8)-[:BELONGS_TO]->(n10)
```

#### Constraints

```cypher
CREATE CONSTRAINT professor IF NOT EXISTS FOR (p:professor) REQUIRE p.professor_id IS UNIQUE;
CREATE CONSTRAINT department IF NOT EXISTS FOR (d:department) REQUIRE d.department_id IS UNIQUE;
CREATE CONSTRAINT course IF NOT EXISTS FOR (c:course) REQUIRE c.course_id IS UNIQUE;
CREATE CONSTRAINT student IF NOT EXISTS FOR (s:student) REQUIRE s.student_id IS UNIQUE;
```

```cypher
MATCH (p:professor)-[:IS_MEMBER_OF]->(d:department {department_id:"D001"})
return p, d
```

```cypher
MATCH (s:student) -[:ENROLLED_IN {semester:"2024-S2"}]->(c:course{course_id:"C002"})
RETURN s,c;
```

```cypher
MATCH(s:student)
WHERE s.age < 25
RETURN s;
```

```cypher

```