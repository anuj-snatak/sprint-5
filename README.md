
#  Jenkins Shared Library: Documentation

## Author Details

| Author      | Created on | Version   | Last updated by | Internal Reviewer |
|-------------|------------|-----------|------------------|--------------------|
| Anuj Jain   | 17-07-25   | version 1 | N/A              | Prashant           |


##  Table of Contents

1. [Overview](#1-overview)
2. [Why Use Shared Libraries](#2-why-use-shared-libraries)
3. [Advantages](#3-advantages)
4. [Disadvantages](#4-disadvantages)
5. [Directory Structure](#5-directory-structure)
6. [Workflow](#6-workflow)
7. [Key Concepts — `vars/` vs `src/`](#7-key-concepts--varsvs-src)
8. [Best Practices](#8-best-practices)
9. [Sample Usage](#9-sample-usage)
10. [Conclusion](#10-conclusion)
11. [Contact Information](#11-contact-information)
12. [References](#12-references)

---

## 1.  Overview

**Jenkins Shared Libraries** allow DevOps teams to extract common pipeline code into a reusable Git repository. This makes Jenkinsfiles modular, clean, and DRY (Don't Repeat Yourself). These libraries are written in Groovy and follow a specific directory structure to expose pipeline steps or utility classes.

---

## 2.  Why Use Shared Libraries

Without shared libraries, each project maintains its own Jenkinsfile logic. As teams grow, this leads to:

* Code duplication
* Inconsistent CI/CD patterns
* Hard-to-maintain pipelines

Using shared libraries solves these problems by:

* Promoting reusability
* Enforcing standards
* Centralizing updates
* Enabling version control

---

## 3.  Advantages

| Benefit                | Description                                                             |
| ---------------------- | ----------------------------------------------------------------------- |
|  **Reusable**        | Define once, use across projects.                                       |
|  **Standardized**    | Uniform CI/CD across teams.                                             |
|  **Clean Pipelines** | Jenkinsfiles stay short and readable.                                   |
|  **Version Control** | Use tags/branches for different versions.                               |
|  **Testable Code**   | Integrate unit testing using `JenkinsPipelineUnit`.                     |
|  **Secure**          | Secrets, sensitive logic can be abstracted away from project pipelines. |

---

## 4.  Disadvantages

| Limitation                 | Description                                                                |
| -------------------------- | -------------------------------------------------------------------------- |
|  **Initial Complexity**  | Requires Jenkins config and directory setup.                               |
|  **Debugging Difficult** | Harder to debug abstracted logic compared to inline pipelines.             |
|  **Learning Curve**      | Requires understanding of Groovy, Jenkins internals, directory structure.  |
|  **Strict Versioning**   | Breaking changes in shared lib can affect many jobs if not versioned well. |

---

## 5.  Directory Structure

```
(shared-library-repo)
├── vars/
│   └── deployApp.groovy      # Global pipeline steps
│   └── deployApp.txt         # Optional documentation
├── src/
│   └── org/
│       └── company/
│           └── utils/
│               └── DockerHelper.groovy
├── resources/
│   └── org/company/templates/email.html
├── test/
│   └── org/company/DockerHelperTest.groovy
└── README.md
```

---

## 6.  Workflow

### Step-by-Step

1. **Configure Shared Lib in Jenkins:**

   * Go to **Manage Jenkins → Global Pipeline Libraries**
   * Add:

     * Name: `my-shared-lib`
     * SCM: Git
     * Repo URL + credentials
     * Default version: `main` or tagged version

2. **Use in Jenkinsfile:**

   ```groovy
   @Library('my-shared-lib@v1.0') _
   ```

3. Jenkins loads:

   * `vars/`: exposed global steps
   * `src/`: compiled Groovy classes
   * `resources/`: used via `libraryResource`

---

## 7.  Key Concepts — `vars/` vs `src/`

| Feature        | `vars/`                           | `src/`                                       |
| -------------- | --------------------------------- | -------------------------------------------- |
| Purpose        | Global pipeline steps (DSL-style) | Groovy classes (helpers, logic separation)   |
| Callable From  | Directly in Jenkinsfile           | Only from other `vars/` or `src/`            |
| Structure      | Flat (filename = function name)   | Package-based (like Java)                    |
| Use Case       | `buildApp()`, `deployApp()`, etc. | `new DockerHelper().runBuild()`              |
| Visibility     | Public API                        | Internal logic                               |
| Syntax Example | `def call(...) { ... }`           | `class DockerHelper { def buildImage() {} }` |

---

## 8.  Best Practices

*  Use **semantic versioning** for libraries (e.g., `v1.2.3`)
*  Keep `vars/` simple; move complex logic to `src/`
*  Use `libraryResource` to fetch files from `resources/`
*  Add `.txt` docs for each global step
*  Include unit tests with [JenkinsPipelineUnit](https://github.com/jenkinsci/JenkinsPipelineUnit)
*  Keep libraries **modular and loosely coupled**
*  Maintain **CHANGELOG.md** for traceability
*  Use `@Field` if you want to persist variables across multiple calls in `vars/`

---

## 9.  Sample Usage

**In Jenkinsfile:**

```groovy
@Library('my-shared-lib@v1.0') _

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                buildApp('java')
            }
        }
        stage('Deploy') {
            steps {
                deployApp('prod')
            }
        }
    }
}
```

**`vars/deployApp.groovy`:**

```groovy
def call(String env) {
    echo "Deploying to ${env}"
    def helper = new org.company.utils.DeployHelper()
    helper.deploy(env)
}
```

**`src/org/company/utils/DeployHelper.groovy`:**

```groovy
package org.company.utils

class DeployHelper {
    def deploy(String env) {
        println "Deployment logic for ${env} goes here."
    }
}
```

---

## 10.  Conclusion

Jenkins Shared Libraries bring **modularity, reusability, maintainability**, and **enterprise-level scalability** to your CI/CD pipelines. By clearly separating public pipeline steps (`vars/`) and internal logic (`src/`), teams can write cleaner, testable, and versioned automation workflows.

---

## Contact Information

| Name      | Email Address                                               |
| --------- | ----------------------------------------------------------- |
| Anuj Jain | [anuj.jain@mygurukulam.co](mailto:anuj.jain@mygurukulam.co) |


Sure bhai! Neeche section 12 **hyperlinked markdown format** me diya gaya hai. Ye GitHub README.md, Notion, Confluence, or any markdown-supported editor me **perfectly render** hoga:

---

## 12. References

* [Jenkins Official Docs – Shared Libraries](https://www.jenkins.io/doc/book/pipeline/shared-libraries/)
* [JenkinsPipelineUnit (Testing) – GitHub](https://github.com/jenkinsci/JenkinsPipelineUnit)
* [Groovy Language Docs – groovy-lang.org](https://groovy-lang.org)
* [Example Repo – Jenkins Shared Lib Example](https://github.com/jenkinsci/workflow-cps-global-lib-plugin)


