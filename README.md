# QuizApp - Spring Boot + PostgreSQL Setup Guide

This README contains step-by-step guidance to set up and run a simple Quiz App backend using Java, Spring Boot, and PostgreSQL.

---

## ✅ Technologies Used

* Java 17 or above
* Spring Boot 3.5.0
* Maven (Apache Maven 3.9.10)
* PostgreSQL

---

## 📁 Project Structure

```
quizapp/
├── src/
│   └── main/java/com/nithesh/quizapp/
│       ├── QuizappApplication.java
│       ├── QuestionController.java
│       ├── Question.java
│       ├── QuestionRepository.java
│       └── QuestionService.java
├── src/main/resources/
│   └── application.properties
└── pom.xml
```

---

## ⚙️ Step-by-Step Setup

### 1. 📦 Generate Spring Boot Project

* Go to [https://start.spring.io/](https://start.spring.io/)
* Choose:

  * Project: Maven
  * Language: Java
  * Spring Boot: 3.5.0
  * Dependencies:

    * Spring Web
    * Spring Data JPA
    * PostgreSQL Driver
* Click **Generate**, then unzip the downloaded project.

### 2. 🧱 Create PostgreSQL Table & Insert Data

Run the following SQL in your PostgreSQL database:

```sql
CREATE TABLE questions (
    id SERIAL PRIMARY KEY,
    category VARCHAR(100) NOT NULL,
    difficulty_level VARCHAR(50) NOT NULL,
    question_title TEXT NOT NULL,
    option1 TEXT NOT NULL,
    option2 TEXT NOT NULL,
    option3 TEXT NOT NULL,
    option4 TEXT NOT NULL,
    right_answer TEXT NOT NULL
);

INSERT INTO questions (category, difficulty_level, question_title, option1, option2, option3, option4, right_answer) VALUES
('Science', 'Easy', 'What planet is known as the Red Planet?', 'Earth', 'Mars', 'Jupiter', 'Saturn', 'Mars'),
('Math', 'Medium', 'What is the square root of 144?', '10', '11', '12', '13', '12'),
... (other sample rows)
```

### 3. 🛠️ Configure application.properties

In `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/your_db_name
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

### 4. 🧩 Create Java Files

#### `Question.java`

```java
@Entity
public class Question {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    private String category;
    private String difficultyLevel;
    private String questionTitle;
    private String option1;
    private String option2;
    private String option3;
    private String option4;
    private String rightAnswer;
    // Getters and setters
}
```

#### `QuestionRepository.java`

```java
public interface QuestionRepository extends JpaRepository<Question, Integer> {}
```

#### `QuestionService.java`

```java
@Service
public class QuestionService {
    @Autowired
    private QuestionRepository questionRepository;

    public List<Question> getAllQuestions() {
        return questionRepository.findAll();
    }
}
```

#### `QuestionController.java`

```java
@RestController
@RequestMapping("question")
public class QuestionController {
    @Autowired
    private QuestionService questionService;

    @GetMapping("allQuestions")
    public List<Question> getAllQuestions() {
        return questionService.getAllQuestions();
    }
}
```

### 5. 🔧 Install & Set Up Maven (if needed)

* Download `apache-maven-3.9.10-bin.zip` from Maven’s official site
* Extract to a folder (e.g. `C:\Program Files\Apache\Maven`)
* Add Maven `bin` folder to your **System Environment Variables > Path**
* Run `mvn -v` to verify installation

### 6. 🚀 Run the App

From the project root:

```bash
mvn clean install
mvn spring-boot:run
```

You should see:

```
Tomcat started on port(s): 8080
```

### 7. 🌐 Test API Endpoint

Open browser or Postman:

```
http://localhost:8080/question/allQuestions
```

You will see all questions fetched from your database.

---

## 📌 Common Issues & Fixes

* ❌ **Filename mismatch**: `Question` class must be in `Question.java`, not `Qustion.java`
* ❌ **Internet/DNS error**: If Maven can't download dependencies, check network or set DNS to `8.8.8.8`
* ❌ **Database errors**: Check your DB name, user, and password in `application.properties`

---

## 🏁 Next Goals

* Add more endpoints like `question/category/{name}`
* Add quiz submission & scoring API
* Build a frontend using React or HTML/JS

---

Let me know if you need help on the next steps! 🚀
