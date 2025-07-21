
#  Jenkins Shared Library: Complete Beginner-Friendly Documentation

##  Table of Contents

1. [What is a Shared Library?](#what-is-a-shared-library)
2. [Why Use Shared Libraries?](#why-use-shared-libraries)
3. [Structure of Shared Library](#structure-of-shared-library)
4. [Creating a Shared Library](#creating-a-shared-library)
5. [Using Shared Library in Jenkinsfile](#using-shared-library-in-jenkinsfile)
6. [Best Practices](#best-practices)
7. [Conclusion](#conclusion)

---

## What is a Shared Library?

A **Jenkins Shared Library** is a reusable set of Groovy scripts and steps that can be shared across multiple Jenkins pipelines. Instead of duplicating common pipeline logic across many Jenkinsfiles, you can write it once in a shared library and call it anywhere.

Think of it as a **custom plugin or module** for Jenkins pipelines.

---

## Why Use Shared Libraries?

| Advantage          | Description                                     |
| ------------------ | ----------------------------------------------- |
|  DRY Code         | Write once, use in multiple pipelines           |
|  Maintainability | Easy to fix/change logic in one place           |
|  Modularity      | Better structure for CI/CD pipelines            |
|  Collaboration   | Team members can contribute to a common library |

---

##  Structure of Shared Library

Here's how a shared library project typically looks:

```
(root)
├── vars/
│   └── hello.groovy       # Simple step function
├── src/
│   └── org/example/
│       └── MyUtils.groovy # Helper classes
├── resources/
│   └── templates/
│       └── message.txt    # External resources like text, yaml, etc.
└── README.md
```

* `vars/` → For globally accessible steps like `hello()`
* `src/` → For classes and functions (like Java-style)
* `resources/` → Templates, configs, etc.
* `README.md` → Project information

---

## 🔹 Creating a Shared Library

### Step 1: Create a Git Repo

Create a Git repo (e.g., `jenkins-shared-lib`) and structure it as shown above.

### Step 2: Create a Sample Step in `vars/hello.groovy`

```groovy
def call(String name = 'Stranger') {
    echo "Hello, ${name} from Shared Library!"
}
```

### Step 3: Add a Class in `src/org/example/MyUtils.groovy`

```groovy
package org.example

class MyUtils implements Serializable {
    def steps

    MyUtils(steps) {
        this.steps = steps
    }

    def sayHello(name) {
        steps.echo "Hello from MyUtils, ${name}"
    }
}
```

---

## 🔹 Using Shared Library in Jenkinsfile

### Step 1: Configure Library in Jenkins

Go to:
`Manage Jenkins` → `Configure System` → `Global Pipeline Libraries`

* Name: `shared-lib`
* Source Code Management: Git
* Project Repository: `https://github.com/yourorg/jenkins-shared-lib.git`
* Default version: `main` or a tag
* Load implicitly:  (unchecked)
* Allow default version:  (checked)

### Step 2: Use It in `Jenkinsfile`

```groovy
@Library('shared-lib') _

hello('Anuj')

def util = new org.example.MyUtils(this)
util.sayHello('Jenkins')
```

---

## 🔹 Best Practices

| Practice                | Explanation                                                   |
| ----------------------- | ------------------------------------------------------------- |
|  Organize Code        | Use `vars/` for simple steps, `src/` for complex logic        |
|  Test Steps           | Use unit testing for groovy classes using JenkinsPipelineUnit |
|  Use Versioning | Tag versions of library for consistent behavior               |
|  Secure Usage         | Avoid hardcoded credentials, use Jenkins credentials store    |
|  Document             | Always maintain README with usage examples                    |

---

##  Conclusion

Jenkins Shared Libraries are a powerful way to **modularize**, **re-use**, and **scale** your Jenkins pipelines. They bring structure and maintainability into CI/CD projects — especially when teams grow.

---
## Contact Information

| Name      | Email Address                                               |
| --------- | ----------------------------------------------------------- |
| Anuj Jain | [anuj.jain@mygurukulam.co](mailto:anuj.jain@mygurukulam.co) |

---


