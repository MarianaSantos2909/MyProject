-- =========================================================
-- Questions:
-- =========================================================

-- 1. List all students

SELECT students.full_name
FROM students;

-- 2. Show students and their departments

SELECT students.full_name, departments.name
   FROM students INNER JOIN departments ON students.department_id = departments.id;

-- 3. Count students by status

SELECT COUNT (DISTINCT final_status)
FROM enrollments;

-- 4. List courses with department name

SELECT courses.title, departments.name
   FROM courses INNER JOIN departments ON courses.department_id = departments.id;

-- 5. Show enrollments with student and course

SELECT 
    s.full_name AS student_name,
    c.title AS course_title,
    e.final_status
FROM enrollments e
JOIN students s ON e.student_id = s.id
JOIN course_offerings co ON e.offering_id = co.id
JOIN courses c ON co.course_id = c.id;

-- 6. Average grade by course offering

SELECT 
    co.id AS offering_id,
    c.title AS course_title,
    ROUND(AVG(g.grade), 2) AS average_grade
FROM grades g
JOIN assignments a ON g.assignment_id = a.id
JOIN course_offerings co ON a.offering_id = co.id
JOIN courses c ON co.course_id = c.id
GROUP BY co.id, c.title
ORDER BY average_grade;

-- 7. Students with no grades yet

SELECT 
    s.full_name
FROM students s
LEFT JOIN grades g ON s.id = g.student_id
WHERE g.id IS NULL;

-- 8. Courses with more than 2 enrolled students

SELECT 
    c.title AS course_title,
    COUNT(e.id) AS total_enrolled
FROM enrollments e
JOIN course_offerings co ON e.offering_id = co.id
JOIN courses c ON co.course_id = c.id
GROUP BY c.title
HAVING COUNT(e.id) > 2;