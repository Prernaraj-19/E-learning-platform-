<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Online Courses</title>
    <link rel="stylesheet" href="styles.css" />
</head>
<body>
    <header>
        <h1>Online Learning Platform</h1>
    </header>

    <main>
        <section id="courses-section">
            <h2>Available Courses</h2>
            <ul id="courses-list"></ul>
        </section>

        <section id="course-details" class="hidden">
            <button id="back-btn">← Back to Courses</button>
            <h2 id="course-title"></h2>
            <p id="course-description"></p>
            <p><strong>Instructor:</strong> <span id="course-instructor"></span></p>
            <p><strong>Price:</strong> $<span id="course-price"></span></p>
        </section>
    </main>

    <script src="script.js"></script>
</body>
</html>
// Sample data simulating courses from your database
const courses = [
    {
        course_id: 1,
        title: "Introduction to Python",
        description: "Learn Python basics including syntax, data types, and control structures.",
        instructor: "Dr. Emily Brown",
        price: 49.99
    },
    {
        course_id: 2,
        title: "Web Development with HTML & CSS",
        description: "Build responsive websites using HTML and CSS.",
        instructor: "Dr. Emily Brown",
        price: 39.99
    }
];

const coursesList = document.getElementById('courses-list');
const courseDetailsSection = document.getElementById('course-details');
const coursesSection = document.getElementById('courses-section');

const courseTitle = document.getElementById('course-title');
const courseDescription = document.getElementById('course-description');
const courseInstructor = document.getElementById('course-instructor');
const coursePrice = document.getElementById('course-price');
const backBtn = document.getElementById('back-btn');

// Function to render the list of courses
function renderCourses() {
    coursesList.innerHTML = '';
    courses.forEach(course => {
        const li = document.createElement('li');
        li.textContent = course.title;
        li.addEventListener('click', () => showCourseDetails(course));
        coursesList.appendChild(li);
    });
}

// Function to show course details
function showCourseDetails(course) {
    courseTitle.textContent = course.title;
    courseDescription.textContent = course.description;
    courseInstructor.textContent = course.instructor;
    coursePrice.textContent = course.price.toFixed(2);

    coursesSection.classList.add('hidden');
    courseDetailsSection.classList.remove('hidden');
}

// Back button event
backBtn.addEventListener('click', () => {
    courseDetailsSection.classList.add('hidden');
    coursesSection.classList.remove('hidden');
});

// Initialize page
renderCourses();
* {
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  margin: 0;
  background-color: #f9f9f9;
  color: #333;
}

header {
  background-color: #007bff;
  color: white;
  padding: 1rem;
  text-align: center;
}

main {
  max-width: 900px;
  margin: 2rem auto;
  padding: 0 1rem;
}

h1, h2 {
  margin-bottom: 1rem;
}

#courses-list {
  list-style: none;
  padding: 0;
}

#courses-list li {
  background-color: white;
  margin-bottom: 1rem;
  padding: 1rem;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  transition: background-color 0.3s ease;
}

#courses-list li:hover {
  background-color: #e9f0ff;
}

#course-details {
  background-color: white;
  padding: 1rem;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.hidden {
  display: none;
}

#back-btn {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
  border-radius: 3px;
  cursor: pointer;
}

#back-btn:hover {
  background-color: #0056b3;
}
