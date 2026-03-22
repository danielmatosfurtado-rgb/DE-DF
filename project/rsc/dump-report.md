-- =========================================================
-- Questions:
-- =========================================================

-- 1. List all students
    SELECT full_name FROM students; 
    
-- 2. Show students and their departments
    SELECT s.full_name, d.name
    FROM students s
    INNER JOIN departments d ON s.department_id = d.id;

-- 3. Count students by status
    SELECT status, COUNT(id) as Total
    FROM students
    GROUP BY status;

-- 4. List courses with department name
    SELECT c.title, d.name
    FROM courses c
    INNER JOIN departments d ON c.department_id = d.id

-- 5. Show enrollments with student and course
    SELECT s.full_name, c.title, e.enrolled_on
    FROM enrollments e
    INNER JOIN course_offerings cf ON e.offering_id = cf.id
    INNER JOIN courses c ON c.id = cf.course_id
    INNER JOIN students s ON s.id = e.student_id

-- 6. Average grade by course offering
    SELECT cf.id, ROUND(AVG(g.grade), 2) AS average_grade
    FROM course_offerings cf
    INNER JOIN assignments a ON a.offering_id = cf.id
    INNER JOIN grades g ON g.assignment_id = a.id
    GROUP BY cf.id
    ORDER BY cf.id;

-- 7. Students with no grades yet
    SELECT s.full_name
    FROM students s
    LEFT JOIN grades g ON s.id = g.student_id
    WHERE g.grade is NULL
    
-- 8. Courses with more than 2 enrolled students
    SELECT c.title, COUNT(e.student_id)
    FROM courses c
    INNER JOIN course_offerings cf ON cf.course_id = c.id
    INNER JOIN enrollments e ON e.offering_id = cf.id
    GROUP BY c.title
    HAVING COUNT(e.student_id) > 2
