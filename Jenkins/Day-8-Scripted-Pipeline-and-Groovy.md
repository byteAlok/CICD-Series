# 🧩 Day 08 – Scripted Pipeline & Groovy Fundamentals

## 📌 1. Jenkins Pipeline – Two Major Approaches

Jenkins provides two major approaches for writing Pipelines:

1. **Declarative Pipeline**
2. **Scripted Pipeline**

Both approaches are used to automate CI/CD processes, but they differ in syntax, structure, and programming flexibility.

### 🔹 1. Declarative Pipeline

Declarative Pipeline provides a structured and easy-to-understand way to define Jenkins Pipelines.

It uses predefined blocks and directives, such as:

- `pipeline`
- `agent`
- `stages`
- `stage`
- `steps`
- `post`
- `when`

### 🔹 2. Scripted Pipeline

Scripted Pipeline is based on the Groovy programming language.

It provides more flexibility by allowing developers to control Pipeline execution using programming logic.

Some commonly used elements include:

- `node`
- `stage`
- Variables
- Conditions
- Loops
- Methods
- Closures
- Exception Handling

### 📊 Declarative vs Scripted

| Feature | Declarative | Scripted |
|---|---|---|
| Syntax | Structured | Groovy-based |
| Programming Style | Predefined structure | Programming-oriented |
| Learning Difficulty | Easy | Comparatively difficult |
| Flexibility | Structured approach | More flexible |
| Variables | Supported | Supported |
| Conditions | `when` | `if-else` |
| Error Handling | `post`, `catchError`, etc. | `try-catch-finally` |
| Execution Block | `pipeline {}` | `node {}` |
| Best Use | Standard CI/CD Pipelines | Complex Pipeline logic |

**Important:** Both approaches are useful. Declarative Pipeline is generally preferred for standard CI/CD workflows because it is easier to read and maintain. Scripted Pipeline is useful when more control over execution logic is required.

---

## 📜 2. Declarative Pipeline – Quick Revision

We have already used Declarative Pipeline throughout Day 01 to Day 07.

Let's quickly revise it with one example. We will not explain this code in detail because we have already covered its main concepts.

### 💻 Example – Declarative Pipeline

```groovy
pipeline {
    agent any

    environment {
        APP_NAME = 'MyApplication'
        VERSION = '1.0.0'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code'
            }
        }

        stage('Build') {
            steps {
                echo "Building ${APP_NAME}"
                sh 'echo Build Completed'
            }
        }

        stage('Testing') {
            steps {
                echo 'Running Automated Tests'
                sh 'echo Tests Passed'
            }
        }

        stage('Validation') {
            steps {
                script {
                    def validationPassed = true

                    if (!validationPassed) {
                        error 'Validation Failed'
                    }

                    echo 'Validation Successful'
                }
            }
        }

        stage('Approval') {
            steps {
                input message: 'Approve Deployment?', ok: 'Deploy'
            }
        }

        stage('Deployment') {
            when {
                branch 'main'
            }

            steps {
                echo "Deploying ${APP_NAME} Version ${VERSION}"
                sh 'echo Deployment Completed'
            }
        }
    }

    post {
        success {
            echo 'Pipeline Successful'
        }

        failure {
            echo 'Pipeline Failed'
        }

        always {
            echo 'Pipeline Execution Finished'
        }
    }
}
```

### 🔍 Understanding the `when` Directive

The `when` directive allows Jenkins to decide whether a particular stage should execute.

In our example:

```groovy
when {
    branch 'main'
}
```

The Deployment stage runs only when the current branch is `main`.

If the Pipeline runs on another branch, Jenkins skips this stage.

**Important:** The `branch` condition is intended for Multibranch Pipelines, where Jenkins provides branch information. A regular Pipeline job may need an `expression` condition based on its configured branch information instead.

Some other commonly used `when` conditions are:

| Condition | Purpose |
|---|---|
| `branch 'main'` | Runs a stage on the specified branch |
| `environment` | Checks an environment variable |
| `expression` | Evaluates a Groovy expression |
| `changeRequest` | Checks whether the build is for a change request |
| `tag` | Checks the current tag |
| `buildingTag()` | Checks whether the build is running for a tag |

Example using an environment condition:

```groovy
when {
    environment name: 'DEPLOY_ENV', value: 'Production'
}
```

This stage runs only when `DEPLOY_ENV` has the value `Production`.

**Note:** The `when` directive is evaluated before the stage executes. We will focus on Scripted Pipeline from this point onward.

---

# 🐍 3. What is Groovy?

Groovy is a high-level, object-oriented programming language that runs on the Java Virtual Machine (JVM).

Its syntax is similar to Java, but it provides additional features that make writing code simpler and more flexible.

### 🔍 Important Features of Groovy

- ☕ Runs on the Java Virtual Machine.
- 🧩 Supports Java libraries and classes.
- 🔄 Supports object-oriented programming.
- 📦 Supports dynamic and static typing.
- 🔤 Provides string interpolation.
- 📋 Provides collections such as Lists and Maps.
- ⚙️ Supports closures and functional programming features.
- 🚀 Requires less boilerplate code than Java for many common tasks.

### 📌 Groovy in Jenkins

Jenkins uses Groovy-based Pipeline syntax to control build and deployment processes.

Example:

```groovy
node {
    stage('Build') {
        echo 'Building Application'
    }
}
```

Here:

- `node` is a Jenkins Pipeline step.
- `stage` is a Jenkins Pipeline step.
- `echo` is a Jenkins Pipeline step.
- Curly braces `{ }` are part of Groovy syntax.

**Important:** A Jenkins Scripted Pipeline is not simply a standalone Groovy program. It runs in the Jenkins Pipeline environment, which provides additional steps such as `node`, `stage`, `echo`, and `sh`.

---

# 🔤 4. Groovy Basic Syntax

Before writing larger Scripted Pipelines, let's understand some important Groovy programming concepts.

## 4.1 Variables

In Groovy, we can use the `def` keyword to declare variables.

```groovy
def name = 'Alok'
def age = 23
def salary = 40000
def isDeveloper = true

println name
println age
println salary
println isDeveloper
```

### 🔍 Explanation

- `def` – Declares a variable.
- `name` – Stores a String value.
- `age` – Stores an Integer value.
- `salary` – Stores a numeric value.
- `isDeveloper` – Stores a Boolean value.
- `println` – Prints output to the console.

Groovy automatically determines the variable's type from its assigned value when using `def`.

We can also declare variables using explicit data types:

```groovy
String name = 'Alok'
int age = 23
double salary = 40000.50
boolean active = true
```

---

## 4.2 Strings and String Interpolation

Groovy supports both single-quoted and double-quoted strings.

```groovy
def name = 'Alok'

def message = "Hello ${name}"

println message
```

Output:

```text
Hello Alok
```

### 🔍 Difference Between Single and Double Quotes

```groovy
def name = 'Alok'

println 'Hello ${name}'
println "Hello ${name}"
```

Output:

```text
Hello ${name}
Hello Alok
```

**Important:** `${variable}` interpolation works with double-quoted GStrings, not ordinary single-quoted Strings.

---

## 4.3 List

A List is a collection used to store multiple values.

```groovy
def subjects = [
    'Java',
    'Python',
    'AWS',
    'DevOps'
]

println subjects[0]
println subjects[1]

subjects.add('Docker')

println subjects
```

Output:

```text
Java
Python
[Java, Python, AWS, DevOps, Docker]
```

List indexing starts from `0`.

---

## 4.4 Map

A Map stores data in key-value pairs.

```groovy
def employee = [
    name: 'Alok',
    age: 23,
    role: 'DevOps Engineer'
]

println employee.name
println employee.age

println employee['role']
```

Output:

```text
Alok
23
DevOps Engineer
```

Each key in a Map is associated with a value.

---

## 4.5 Conditions – if-else

Groovy uses curly braces for conditional statements, similar to Java and JavaScript.

```groovy
def marks = 75

if (marks >= 60) {
    println 'First Division'
}
else {
    println 'Second Division'
}
```

Output:

```text
First Division
```

---

## 4.6 Switch Statement

We can use `switch` when we need to check multiple possible values.

```groovy
def environment = 'Production'

switch (environment) {
    case 'Development':
        println 'Development Server'
        break

    case 'Production':
        println 'Production Server'
        break

    default:
        println 'Unknown Environment'
}
```

Output:

```text
Production Server
```

---

## 4.7 Loops

Loops allow us to execute the same block of code multiple times.

### 🔹 For Loop

```groovy
def tools = ['Git', 'Docker', 'Jenkins', 'AWS']

for (tool in tools) {
    println tool
}
```

Output:

```text
Git
Docker
Jenkins
AWS
```

### 🔹 While Loop

```groovy
def count = 1

while (count <= 5) {
    println count
    count++
}
```

Output:

```text
1
2
3
4
5
```

### 🔹 Each Loop (Collection Iteration)

Groovy also provides the `each()` method to iterate over collections such as Lists and Maps.

It is commonly used when we want to perform an operation on every element of a collection.

```groovy
def tools = ['Git', 'Docker', 'Jenkins', 'AWS']

tools.each { tool ->
    println tool
}
```

Output:

```text
Git
Docker
Jenkins
AWS
```

### 🔍 Understanding `each()`

```groovy
tools.each { tool ->
    println tool
}
```

- `each()` – Iterates over every element in the collection.
- `{ }` – Defines a Closure.
- `tool` – Receives the current element.
- `println tool` – Prints the current element.

We can also use `each()` with a Map:

```groovy
def employee = [
    name: 'Alok',
    role: 'DevOps Engineer',
    city: 'Hyderabad'
]

employee.each { key, value ->
    println "${key}: ${value}"
}
```

Output:

```text
name: Alok
role: DevOps Engineer
city: Hyderabad
```

**Important:** Groovy does not have a separate `foreach` keyword like some other languages. Instead, `each()` is commonly used for collection iteration. The `for-in` loop is another way to iterate over collections.

---

## 4.8 Methods

Methods are reusable blocks of code that perform a particular task.

```groovy
def greet(name) {
    return "Hello ${name}"
}

def result = greet('Alok')

println result
```

Output:

```text
Hello Alok
```

### 🔍 Explanation

- `def` – Declares a method.
- `greet` – Method name.
- `name` – Method parameter.
- `return` – Returns a value.
- `greet('Alok')` – Calls the method.

---

## 4.9 Closures

A Closure is a block of code that can be stored in a variable and executed later.

Closures are an important feature of Groovy and are frequently used in Scripted Pipelines.

```groovy
def greet = {
    println 'Hello Jenkins'
}

greet()
```

Output:

```text
Hello Jenkins
```

### 🔹 Closure with Parameter

```groovy
def greet = { name ->
    println "Hello ${name}"
}

greet('Alok')
```

Output:

```text
Hello Alok
```

Closures are especially useful when working with collection methods and parallel execution in Jenkins.

---

## 4.10 Exception Handling

Groovy supports `try-catch-finally`, similar to Java.

```groovy
try {
    int result = 10 / 0
    println result
}
catch (Exception e) {
    println "Error: ${e.message}"
}
finally {
    println 'Execution Finished'
}
```

The exception is caught, and the `finally` block runs afterward.

### 🔍 Explanation

| Keyword | Purpose |
|---|---|
| `try` | Contains code that may throw an exception |
| `catch` | Handles the exception |
| `finally` | Executes cleanup code |
| `throw` | Manually throws an exception |

---

# 🔄 5. Groovy vs Other Programming Languages

Groovy may look familiar if you have already worked with JavaScript, Python, Java, or C#.

However, Groovy has its own syntax and execution model.

Groovy supports dynamic typing and runs on the JVM.

## 5.1 Groovy vs JavaScript

Both languages use curly braces and support variables, functions, and closures.

### JavaScript

```javascript
const name = 'Alok';

function greet(user) {
    return `Hello ${user}`;
}

console.log(greet(name));
```

### Groovy

```groovy
def name = 'Alok'

def greet = { user ->
    return "Hello ${user}"
}

println greet(name)
```

**Key Differences:**

- JavaScript uses `const`, `let`, and `var` for variable declarations.
- Groovy commonly uses `def`.
- JavaScript uses backticks for template literals.
- Groovy uses double-quoted GStrings for interpolation.
- Both support closures or functions, but their syntax and execution models differ.

---

## 5.2 Groovy vs Python

Python uses indentation to define code blocks, whereas Groovy uses curly braces `{ }`.

### Python

```python
name = 'Alok'
age = 23

if age >= 18:
    print(f'Hello {name}')
else:
    print('Not Eligible')
```

### Groovy

```groovy
def name = 'Alok'
def age = 23

if (age >= 18) {
    println "Hello ${name}"
}
else {
    println 'Not Eligible'
}
```

**Key Differences:**

- Python uses indentation to define code blocks.
- Groovy uses curly braces.
- Python uses `def` to define functions.
- Groovy can use `def` for both variables and methods.

---

## 5.3 Groovy vs Java

Groovy's syntax is quite similar to Java.

Both are part of the JVM ecosystem, but Groovy often requires less boilerplate code.

### Java

```java
public class Main {

    public static void main(String[] args) {

        String name = "Alok";
        int age = 23;

        if (age >= 18) {
            System.out.println("Eligible");
        }
    }
}
```

### Groovy

```groovy
def name = 'Alok'
def age = 23

if (age >= 18) {
    println 'Eligible'
}
```

**Key Differences:**

- Java commonly uses a class and `main` method as the application entry point.
- Groovy scripts can be executed directly without writing that boilerplate structure.
- Java is statically typed.
- Groovy supports both dynamic and static typing.
- Groovy can use Java classes and libraries.

---

## 5.4 Groovy vs C#

C# also uses curly braces, making its syntax familiar to Java and Groovy developers.

### C#

```csharp
string name = "Alok";
int age = 23;

if (age >= 18)
{
    Console.WriteLine($"Hello {name}");
}
else
{
    Console.WriteLine("Not Eligible");
}
```

### Groovy

```groovy
def name = 'Alok'
def age = 23

if (age >= 18) {
    println "Hello ${name}"
}
else {
    println 'Not Eligible'
}
```

**Key Differences:**

- C# commonly uses explicit types such as `string`, `int`, and `bool`.
- Groovy allows dynamic typing through `def`.
- C# uses `$"..."` for string interpolation.
- Groovy uses `"${...}"` for interpolation.
- C# runs on the .NET runtime, whereas Groovy runs on the JVM.

---

## 📊 5.5 Quick Comparison

| Feature | Groovy | JavaScript | Python | Java | C# |
|---|---|---|---|---|---|
| Runtime | JVM | JS Runtime | Python Runtime | JVM | .NET |
| Typing | Dynamic/Static | Dynamic | Dynamic | Static | Static |
| Variable | `def` | `let`, `const` | Direct assignment | `int`, `String` | `int`, `string` |
| Conditions | `{ }` | `{ }` | Indentation | `{ }` | `{ }` |
| Functions | Methods/Closures | Functions | `def` | Methods | Methods |
| String Interpolation | `${}` | `${}` | `{}` in f-strings | String concatenation or supported templates | `$"..."` |

### 💡 Conclusion

Groovy's syntax will feel familiar if you have worked with Java, JavaScript, or C#.

Its syntax is quite different from Python, mainly because Python uses indentation while Groovy uses curly braces.

Understanding these similarities will make it easier to learn Groovy for Jenkins Scripted Pipelines.

---

# ⚙️ 6. Jenkins Scripted Pipeline

Now that we understand the basic Groovy concepts, let's look at how they are used in Jenkins Scripted Pipelines.

Scripted Pipeline is Jenkins' Groovy-based approach to defining Pipeline execution.

The main execution structure uses the `node` block.

### 📌 Basic Structure

```groovy
node {

    stage('Build') {
        echo 'Building Application'
    }

    stage('Test') {
        echo 'Testing Application'
    }

    stage('Deploy') {
        echo 'Deploying Application'
    }
}
```

### 🔍 Important Jenkins Pipeline Steps

| Step | Purpose |
|---|---|
| `node` | Allocates a Jenkins execution Node |
| `stage` | Divides the Pipeline into logical stages |
| `echo` | Prints a message in the Jenkins console |
| `sh` | Executes a Linux shell command |
| `input` | Requests manual approval |
| `timeout` | Sets a time limit for execution |
| `error` | Fails the Pipeline with an error |
| `parallel` | Executes multiple tasks concurrently |
| `checkout` | Checks out source code |

**Important:** These are Jenkins Pipeline steps, not built-in Groovy functions.

---

# 🚀 7. Complete Scripted Pipeline – Practical Example

Now let's create a complete Scripted Pipeline that demonstrates a typical CI/CD workflow.

### 🎯 Pipeline Workflow

1. Initialization
2. Build
3. Testing
4. Parallel Testing
5. Validation
6. Manual Approval
7. Deployment
8. Error Handling
9. Cleanup

### 💻 Complete Scripted Pipeline Code

```groovy
node('slave1') {

    // 1. VARIABLES ------------------

    def appName = 'MyApplication'
    def environment = 'Production'
    def buildVersion = '1.0.0'

    def buildStatus = 'NOT_STARTED'

    // List
    def testSuites = [
        'Unit Testing',
        'Integration Testing',
        'API Testing'
    ]

    // Map
    def appConfig = [
        language : 'Java',
        framework: 'Spring Boot',
        buildTool: 'Maven'
    ]

    // 2. REUSABLE CLOSURES ---------------------

    def printInfo = { message ->
        echo "INFO: ${message}"
    }

    def validateApplication = {
        echo 'Validating Application...'

        if (appName == '') {
            error 'Application Name Missing!'
        }

        echo 'Application Validation Successful'
    }

    // 3. PIPELINE EXECUTION -----------------------

    try {

        // STAGE 1: INITIALIZATION -----------

        stage('Initialization') {

            printInfo("Application: ${appName}")
            printInfo("Version: ${buildVersion}")
            printInfo("Environment: ${environment}")

            echo "Language: ${appConfig.language}"
            echo "Framework: ${appConfig.framework}"
            echo "Build Tool: ${appConfig.buildTool}"

            buildStatus = 'INITIALIZED'
        }

        // STAGE 2: BUILD ------------

        stage('Build') {

            echo 'Starting Application Build'

            sh 'echo Compiling Application'
            sh 'sleep 3'

            echo 'Application Build Completed'

            buildStatus = 'BUILD_SUCCESS'
        }

        // STAGE 3: TEST --------------

        stage('Testing') {

            echo 'Starting Automated Testing'

            for (int i = 0; i < testSuites.size(); i++) {

                echo "Executing: ${testSuites[i]}"

                sh 'sleep 2'

                echo "${testSuites[i]} Completed"
            }

            buildStatus = 'TEST_SUCCESS'
        }

        // STAGE 4: PARALLEL TESTING --------------

        stage('Parallel Testing') {

            def parallelTasks = [

                'Security Scan': {
                    echo 'Running Security Scan'
                    sh 'sleep 3'
                    echo 'Security Scan Completed'
                },

                'Code Quality': {
                    echo 'Running Code Quality Analysis'
                    sh 'sleep 3'
                    echo 'Code Quality Analysis Completed'
                },

                'Dependency Check': {
                    echo 'Checking Dependencies'
                    sh 'sleep 3'
                    echo 'Dependency Check Completed'
                }
            ]

            parallel parallelTasks
        }

        // STAGE 5: VALIDATION ---------------

        stage('Validation') {

            validateApplication()

            echo 'All Validations Passed'
        }

        // STAGE 6: MANUAL APPROVAL ---------------

        stage('Approval') {

            timeout(time: 2, unit: 'MINUTES') {

                input(
                    message: 'Do you want to deploy the application?',
                    ok: 'Approve',
                    submitter: 'admin'
                )

                echo 'Deployment Approval Received'
            }
        }

        // STAGE 7: DEPLOYMENT --------------

        stage('Deployment') {

            echo "Deploying ${appName}"

            echo "Target Environment: ${environment}"

            sh 'echo Deploying Application'
            sh 'sleep 3'

            echo 'Deployment Completed'

            buildStatus = 'DEPLOY_SUCCESS'
        }

    }

    // 4. ERROR HANDLING -----------------------------

    catch (Exception e) {

        buildStatus = 'FAILED'

        echo "ERROR: ${e.getMessage()}"

        throw e
    }

    // 5. CLEANUP -----------------------------

    finally {

        echo '--------- PIPELINE EXECUTION SUMMARY ---------- '

        echo "Application: ${appName}"
        echo "Version: ${buildVersion}"
        echo "Environment: ${environment}"
        echo "Final Status: ${buildStatus}"

        echo 'Cleaning Temporary Files'

        sh 'echo Cleanup Completed'

        echo 'Pipeline Execution Finished'
    }
}
```

---

## 🔍 8. Complete Scripted Pipeline – Code Explanation

### 🧠 1. Node Allocation

```groovy
node('slave1') {
    // Pipeline Code
}
```

- `node` is a Jenkins Pipeline step.
- `slave1` is the label assigned to the Agent.
- Jenkins allocates an eligible Node with the `slave1` label.
- An Executor must be available on that Node.

### 📦 2. Variables

```groovy
def appName = 'MyApplication'
def environment = 'Production'
def buildVersion = '1.0.0'
```

These variables store application-related information.

### 📋 3. List

```groovy
def testSuites = [
    'Unit Testing',
    'Integration Testing',
    'API Testing'
]
```

We store multiple test suites in a single List.

### 🗂️ 4. Map

```groovy
def appConfig = [
    language : 'Java',
    framework: 'Spring Boot',
    buildTool: 'Maven'
]
```

Application configuration is stored as key-value pairs.

### 🔧 5. Closures

```groovy
def printInfo = { message ->
    echo "INFO: ${message}"
}
```

This creates a reusable block of code that can be called multiple times.

### 🔄 6. Loop

```groovy
for (int i = 0; i < testSuites.size(); i++) {
    echo "Executing: ${testSuites[i]}"
}
```

This loop iterates through every element in the List.

### ⚡ 7. Parallel Execution

```groovy
parallel parallelTasks
```

This executes multiple independent tasks concurrently.

In our example, the tasks are:

- Security Scan
- Code Quality
- Dependency Check

### ⏱️ 8. Timeout and Approval

```groovy
timeout(time: 2, unit: 'MINUTES') {
    input(
        message: 'Do you want to deploy?',
        ok: 'Approve',
        submitter: 'admin'
    )
}
```

- `timeout` sets the maximum waiting time.
- `input` requests manual approval.
- `submitter` restricts who can approve the deployment.

### 🚨 9. Exception Handling

```groovy
catch (Exception e) {
    buildStatus = 'FAILED'
    echo "ERROR: ${e.getMessage()}"
    throw e
}
```

If an exception occurs:

- The exception is caught.
- The error message is printed.
- The status variable is updated.
- `throw e` rethrows the exception so Jenkins marks the build as failed or interrupted according to the exception.

### 🧹 10. Finally

```groovy
finally {
    echo 'Pipeline Execution Finished'
}
```

The `finally` block runs after normal completion or an exception, unless execution is forcibly terminated.

It is commonly used for cleanup and summary operations.

---

## ⚠️ 9. Important Practical Notes

- The Agent with the `slave1` label must be online.
- Java must be installed on the Agent.
- A Linux-based Agent is required for the `sh` commands.
- The `admin` value in `submitter` must match an actual Jenkins username with permission to approve.
- Available Executors are required for parallel tasks.
- The `sleep` and `echo` commands in this example are demonstration commands, not actual Maven build or deployment operations.
- `buildStatus` is a manually maintained variable, not Jenkins' official build result.
- Timeouts and manual aborts are Pipeline interruptions. They should not always be treated as ordinary failures. Production Pipelines need careful interruption handling.
- Jenkins Pipeline uses the Groovy CPS execution model, so some advanced Groovy programming patterns have additional restrictions when used in Pipelines.

---

## 📚 10. Quick Revision

| Concept | Purpose |
|---|---|
| `def` | Declares a variable or method |
| `println` | Prints output |
| `List` | Stores multiple values |
| `Map` | Stores key-value data |
| `if-else` | Conditional execution |
| `switch` | Checks multiple conditions |
| `for` | Iterates through values |
| `while` | Repeats while a condition is true |
| `each()` | Iterates over collection elements |
| `return` | Returns a value |
| `Closure` | Reusable block of code |
| `try-catch` | Handles exceptions |
| `node` | Allocates a Jenkins execution Node |
| `stage` | Defines a Pipeline stage |
| `parallel` | Executes branches concurrently |
| `input` | Requests manual approval |
| `timeout` | Sets an execution time limit |

---

## 🎯 11. Day 08 Learning Outcome

After completing Day 08, we understand:

- 🏗️ Declarative and Scripted Pipeline differences.
- 🐍 Introduction to the Groovy programming language.
- 🔤 Variables and Data Types.
- 📋 Lists and Maps.
- 🔀 Conditions and Loops.
- 🔁 Collection iteration using `each()`.
- 🔧 Methods and Closures.
- 🚨 Exception Handling.
- 🔄 Scripted Pipeline structure.
- ⚡ Parallel Execution.
- ⏱️ Timeout and Manual Approval.
- 🚀 Complete Scripted Pipeline example.

---

## 🚀 Next Learning Step

**Day 09 – Jenkinsfile & Pipeline as Code**

In the next session, we will cover Jenkinsfile fundamentals, Declarative and Scripted Pipelines, environment variables, parameters, credentials, and pipeline options. We will also practice GitHub integration, debugging, and essential pipeline steps.
