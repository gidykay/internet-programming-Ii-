#feedback_form.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Feedback Form</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h2>Campaign Feedback Form</h2>
        <form id="feedbackForm" action="submit_feedback.php" method="POST">
            <label for="name">Name:</label>
            <input type="text" id="name" name="name" required>
            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required>
            <label for="feedback">Feedback:</label>
            <textarea id="feedback" name="feedback" rows="4" required></textarea>
            <label for="rating">Rating (1-5):</label>
            <input type="number" id="rating" name="rating" min="1" max="5" required>
            <button type="submit">Submit</button>
        </form>
    </div>
    <script>
        document.getElementById('feedbackForm').addEventListener('submit', function(event) {
            const rating = document.getElementById('rating').value;
            if (rating < 1 || rating > 5) {
                alert('Rating must be between 1 and 5');
                event.preventDefault();
            }
        });
    </script>
</body>
</html>
#
Here’s a CSS animation setup for the feedback form page that adds smooth transitions to the form elements and an animated background effect for a modern, engaging user experience.

feedback_form.html with Animated CSS Styling
html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Feedback Form</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h2>Campaign Feedback Form</h2>
        <form id="feedbackForm" action="submit_feedback.php" method="POST">
            <label for="name">Name:</label>
            <input type="text" id="name" name="name" required>
            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required>
            <label for="feedback">Feedback:</label>
            <textarea id="feedback" name="feedback" rows="4" required></textarea>
            <label for="rating">Rating (1-5):</label>
            <input type="number" id="rating" name="rating" min="1" max="5" required>
            <button type="submit">Submit</button>
        </form>
    </div>
    <script>
        document.getElementById('feedbackForm').addEventListener('submit', function(event) {
            const rating = document.getElementById('rating').value;
            if (rating < 1 || rating > 5) {
                alert('Rating must be between 1 and 5');
                event.preventDefault();
            }
        });
    </script>
</body>
</html>
# styles.css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: Arial, sans-serif;
}

body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background: linear-gradient(135deg, #2196f3, #21cbf3);
    animation: gradientBackground 5s infinite alternate;
}

@keyframes gradientBackground {
    0% { background-color: #2196f3; }
    50% { background-color: #21cbf3; }
    100% { background-color: #2196f3; }
}

.container {
    width: 90%;
    max-width: 500px;
    background: rgba(255, 255, 255, 0.9);
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    transform: scale(0.95);
    animation: popIn 0.5s ease forwards;
}

@keyframes popIn {
    0% { transform: scale(0); opacity: 0; }
    100% { transform: scale(1); opacity: 1; }
}

h2 {
    text-align: center;
    margin-bottom: 20px;
    color: #333;
    font-size: 1.8rem;
    animation: fadeInDown 0.8s ease;
}

@keyframes fadeInDown {
    from { opacity: 0; transform: translateY(-30px); }
    to { opacity: 1; transform: translateY(0); }
}

form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

label {
    font-weight: bold;
    color: #555;
}

input[type="text"],
input[type="email"],
textarea,
input[type="number"] {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    transition: all 0.3s;
}

input[type="text"]:focus,
input[type="email"]:focus,
textarea:focus,
input[type="number"]:focus {
    border-color: #2196f3;
    box-shadow: 0 0 5px rgba(33, 150, 243, 0.5);
}

button {
    padding: 10px;
    background-color: #2196f3;
    color: #fff;
    border: none;
    border-radius: 4px;
    font-size: 1rem;
    cursor: pointer;
    transition: all 0.3s;
}

button:hover {
    background-color: #21cbf3;
    transform: translateY(-2px);
}

button:active {
    transform: translateY(0);
}
#submit_feedback.php
<?php
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $name = $_POST['name'];
    $email = $_POST['email'];
    $feedback = $_POST['feedback'];
    $rating = $_POST['rating'];

    $conn = new mysqli('localhost', 'root', '', 'campaign_feedback');

    if ($conn->connect_error) {
        die("Connection failed: " . $conn->connect_error);
    }

    $sql = "INSERT INTO feedback (name, email, feedback, rating, submission_date) VALUES (?, ?, ?, ?, NOW())";
    $stmt = $conn->prepare($sql);
    $stmt->bind_param("sssi", $name, $email, $feedback, $rating);

    if ($stmt->execute()) {
        echo "Thank you for your feedback!";
    } else {
        echo "Error: " . $stmt->error;
    }

    $stmt->close();
    $conn->close();
}
?>
#view_feedback.php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>View Feedback</title>
    <style>
        table { width: 100%; border-collapse: collapse; }
        th, td { padding: 8px 12px; border: 1px solid #ddd; }
        th { background-color: #f4f4f4; }
    </style>
</head>
<body>
    <h2>Feedback Received</h2>
    <table>
        <tr>
            <th>ID</th>
            <th>Name</th>
            <th>Email</th>
            <th>Feedback</th>
            <th>Rating</th>
            <th>Submission Date</th>
        </tr>

        <?php
        $conn = new mysqli('localhost', 'root', '', 'campaign_feedback');
        
        if ($conn->connect_error) {
            die("Connection failed: " . $conn->connect_error);
        }

        $sql = "SELECT id, name, email, feedback, rating, submission_date FROM feedback ORDER BY submission_date DESC";
        $result = $conn->query($sql);

        if ($result->num_rows > 0) {
            while ($row = $result->fetch_assoc()) {
                echo "<tr>
                        <td>{$row['id']}</td>
                        <td>{$row['name']}</td>
                        <td>{$row['email']}</td>
                        <td>{$row['feedback']}</td>
                        <td>{$row['rating']}</td>
                        <td>{$row['submission_date']}</td>
                    </tr>";
            }
        } else {
            echo "<tr><td colspan='6'>No feedback found</td></tr>";
        }

        $conn->close();
        ?>
    </table>
</body>
</html>
#Database Creation
CREATE DATABASE campaign_feedback;

USE campaign_feedback;

CREATE TABLE feedback (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    feedback TEXT NOT NULL,
    rating INT NOT NULL,
    submission_date DATETIME DEFAULT CURRENT_TIMESTAMP
);
