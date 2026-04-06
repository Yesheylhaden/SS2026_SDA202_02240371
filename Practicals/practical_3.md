# Automated Grading System

## Overview

The requirement of Automated Grading System is to ensure the preciseness, efficiency, accuracy, plagiarism free, monitoring, etc., of the whole Assignment Life Cycle Management process from submission to evaluation using multi-user capability, including students, professors, auditors, etc., and also integrating with other applications such as Turnitin and LMS. Time, energy, and effort get saved with the Automated Grading System since the repetitive grading activity is made automatic and synchronization of results.

Modeling of Automated Grading System is possible through **UML Class and Object Diagrams**, which are going to be used to model some real-time scenarios in the system and how these different actors are going to relate with each other.

---

## UML Class Diagram:

![ClassDiagram](./Diagram/ClassDiagram.drawio.png)

## 1. System Actors (Classes)

### 1.1 Student
- **Description**: An item that acts as a representation of a student engaged in a class.
- **Attributes**:
  - `studentId: String` – The unique identifier of the student.
  - `name: String` – The name of the student.
  - `email: String` – The email address of the student.
- **Methods**:
  - `login()` – The method logs in the student.
  - `submitAssignment()` – Submit an assignment through the system.
  - `viewGrade()` – View the grade received by the student on submitting the assignment.

### 1.2 Professor
- **Description**: A teacher who takes charge of creating and managing assignments along with evaluating student submissions.
- **Attributes**:
  - `professorId: String`: Unique identifier for each professor.
  - `name: String`: Full name of the professor.
  - `department: String`: The department to which the professor is affiliated.
- **Methods**:
  - `login()`: Login method for professors.
  - `createAssignment()`: Create an assignment for the student.
  - `gradeSubmissions()`: Evaluate manual/automatic submission by students.

### 1.3 Administrator
- **Responsibilities**: In charge of handling the user data and accounts.
- **Properties**:
  - `adminId`: String – The ID of the administrator.
  - `name`: String – The name of the administrator.
  - `email`: String – The email of the administrator.
- **Methods**:
  - `login()`: Used to authenticate the login details of the administrator.
  - `manageUsers()`: Used to create, update, and delete users.
  - `generateReports()`: Used to generate multiple types of reports on the system.

### 1.4 State Auditor
- **Description**: An external agent who performs the audit on the system.
- **Properties**:
  - `auditId`: String – The ID for the audit request.
  - `agency`: String – The name of the auditing agency.
- **Methods**:
  - `requestAudit()`: Carries out the audit process to check for compliance.
  - `viewRecords()`: Views the logs and grades within the system.

---

## 2. Supporting Classes

### 2.1 Submission
- **Description**: Represents submission of a student.
- **Properties**:
  - `assignmentId: String` – Refers to the assignment ID.
  - `title: String` – Submission title.
  - `dateTime: String` – Submission datetime.
- **Methods**:
  - `submit()` – Function that submits the completed submission.
  - `updateGrade()` – Method that updates the grade of the submission.

### 2.2 Grades
- **Description**: Stores grades for students’ assignments.
- **Fields**:
  - `assignmentId: String` – Refer to the assignment Id.
  - `title: String` – Assignment title.
  - `datetime: String` – Datetime of grading the assignment.

---

## 3. External Interfaces

### 3.1 TurnitinAPI (Interface)
- **Description**: It is utilized to detect any form of plagiarism in the submitted content.
- **Methods**:
  - `checkPlagiarism()` – It verifies the content against plagiarism.
  - `getSimilarityReport()`

### 3.2 LMSMainframe (Interface)
- **Goal**: Data synchronization with an external learning management system.
- **Methods**:
  - `syncStudentData()` – It loads all the data of the student.
  - `validateEnrollment()` – It validates the enrollment of the student.
  - `storeGrades()` – It stores

## 5. Relationship and Interaction

- **Student ↔ Submission:** Interaction between the student and the submission module (submission creation and submission).
- **Professor → TurnitinAPI:** Professor uses the API to verify plagiarism.
- **Professor → Audit Data:** Professor has access to the audit and grading data of the submission.
- **Administrator → State Auditor:** Information is provided by the administrators to the state auditor. 
- **Administrator → Data:** Administrator manages all the data of the system.
- **Grades ↔ LMSMainframe:** Grades are updated from the external Learning Management System.

## Object Diagram:

![ObjectDiagram](./Diagram/ObjectDiagram.drawio.png)

## 6. Purpose of the Object Diagram

- **Validation:** Validates whether the class relationships can actually be implemented.
- **Debugging:** Shows one particular state of the system (such as grading).
- **Communication:** Illustrates precisely how data is shared between the objects.
- **Test Cases:** Enables the generation of test cases that can be used for testing.

---

## 7. Object Instances (by Type)

### 7.1 User Objects

#### Student Object
| Attribute        | Value           |
|-----------------|-----------------|
| studentId       | "02240371"      |
| name            | "Yeshey Lhaden" |
| email           | "lhaden66@gmail.com" |

**Explanation:** An instance of the student who has enrolled into classes to submit assignment and view grades.

#### Professor Object
| Attribute          | Value               |
|-------------------|---------------------|
| professorId        | "P001"              |
| name              | "Dr. Smith"         |
| reviewQueue       | ["SUB-001"]         |

**Explanation:** Currently Dr. Smith is working on the submission `SUB-001` for review.

### 7.2 Submission Objects

#### Assignment Object
| Attribute        | Value              |
|------------------|--------------------|
| assignmentId     | "AS01"             |
| title            | "Software Design Architecture"       |
| deadline         | "2024-03-16 12:00 am"   |

**Interpretation:** Specific assignment with a deadline at midnight.

#### Submission Object
| Attribute        | Value              |
|------------------|--------------------|
| submissionId     | "SUB01"            |
| submissionTime   | "2024-03-15 23:45:00"     |
| status           | "pending"          |
| codeUrl          | "github/..."      |

**Interpretation:** The assignment was submitted by the student just before the deadline. However, its status is "pending," i.e., not graded yet.

---

### 7.3 Processing Objects

#### AutoGrader Object
| Attribute        | Value              |
|------------------|--------------------|
| graderId         | "AG01"             |
| currentJob       | "SUB-001"          |
| status           | "busy"             |

**Interpretation:** An automatic grader is processing

#### TestCase Object
| Field   | Value             |
|---------|------------------|
| testId  | "TC-01"           |
| input   | "[3,1,2]"          |
| output  | "[1,2,3]"         |

**Explanation:** This is a sorting test case in which the input value `[3,1,2]` must be sorted to `[1,2,3]`.

#### GradingResult Object
| Field   | Value     |
|---------|----------|
| resultId | "GR-001"  |
| score   | 85        |
| message | "2/4 test cases passed" |

**Explanation:** The student earned 85% with only 2 out of 4 test cases passed.

#### AuditLog Object
| Field   | Value                   |
|---------|------------------------|
| logId   | "LOG-001"              |
| action  | "grade"                 |
| dateTime| "2024-03-15 23:47:

---

## 8. Relationship Between Objects & Scenario Trace

### 8.1 Runtime Scenario
Student (Yeshey Lhaden)
-> Submits -> Assignment (AS01)
-> Creates -> Submission (SUB01)
-> Processed By -> AutoGrader (AG01)
-> Uses -> TestCase (TC-01)
-> Generates -> GradingResult (GR-001)
-> Recorded By -> AuditLog (LOG-001)
-> Checked By -> Professor (Dr. Smith)

---

## 9. Comparison: Class Diagram vs. Object Diagram

| Attribute | Class Diagram | Object Diagram |
|-----------|---------------|----------------|
| Purpose | Indicates blueprint of the system, relationships | Indicate snapshot of the system at particular time |
| What does it indicate? | Shows classes, interfaces and abstract types | Shows objects and its properties (instances) with actual values |
| Attributes | Only types (such as int, String) | Actual values (such as 85, “02240371”) |
| Methods/Functions | Method signatures only (such as + login()) | No methods but indicates state of objects |
| Relationships | Shows inheritance, interfaces and multiplicity (such as 1..*) | Indicates relationships between objects |
| Number of entities | Shows all possible classes | Shows specific objects at a particular time |
| Development | For designing database, APIs, and generating code | Helps in testing, debugging and documentation of specific scenarios |
| Stability | Generally **static** | Dynamic |

---

## Conclusion:
The report shows how **Automated Grading System** could be used to improve academic processes through automated grading, consistent data management, and powerful auditing and reporting functionality.