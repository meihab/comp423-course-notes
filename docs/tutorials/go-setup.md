# Setting up a dev container for Go

* Primary author: [Mohammad Saatialsoruji](https://github.com/meihab)
* Reviewer: [Muhammad Fouly](https://github.com/MuhammadDF)

``` py
# This is an example of a code block
# Code blocks are an essential part of project documentation
# We will see these many times throughout the tutorial
print("We love COMP 423!")
```

!!! success
    I have successfully set up admonitions for my partner! - Muhammad Fouly             
    You will see these throughout the tutorial for the purpose of side content without disturbing document flow :)

## Welcome 
This tutorial will provide step-by-step instructions for creating a Development (Dev) container in Go, a programming language created by Google!
## Prerequisites
To proceed with this tutorial, you will need to have the following: 

1. **A GitHub account**: sign up at [GitHub](https://github.com/).
2. **Git installed**: Install from [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
3. **Visual Studio Code**: Download and install from [here](https://code.visualstudio.com/).
4. **Docker installed**: Get Docker [here](https://www.docker.com/products/docker-desktop/).
5. **Basic command-line knowledge**

## Part 1. Let's Start With the Project Setup!
### Creating a Local Directory and Initializing Git
- Open your terminal or command prompt. 
- Create a new directory for you project by running:
``` bash
mkdir go-dev-container
cd go-dev-container
```
- Initialize a new Git repository:
``` bash
git init
```
- Create a README file as follows:
``` bash
echo "# Setting up a Dev Container for Go" > README.md
git add README.md
git commit -m "Initial commit with README"
```

### Creating a GitHub Repository
(A) Log in to GitHub and navigate to the [Create a New Repository](https://github.com/new) page.

(B) Fill in the details as follows:

- **Repository Name:** `go-dev-container`
- **Description:** "Setting up a dev container for Go project."
- **Visibility:** Public

(C) Do not initialize the repository with a README, .gitignore, or license.

(D) Click Create **Repository**.

### Link your Local Repository to GitHub
(A) Add the GitHub repository as a remote:
``` bash
git remote add origin https://github.com/<your-username>/go-dev-container.git
``` 
Replace `<your-username>` with your GitHub username.

(B) Check your default branch name with the subcommand `git branch`. 

!!! Note
    Old versions of git choose the name `master` for the primary branch, but these days `main` is the standard name. You can rename your default branch to main using the following command: `git branch -M main`

(C) Push your local commits to the GitHub repository:
``` bash
git push --set-upstream origin main
``` 

(D) Back in your web browser, refresh your GitHub repository to see that the same commit you made locally has now been pushed to remote. You can use git log locally to see the commit ID and message which should match the ID of the most recent commit on GitHub. This is the result of pushing your changes to your remote repository.

## Part 2. Setting Up the Development Environment (CHANGE)

### What is a Development (Dev) Container?

A dev container ensures that you are using the same development tools no matter which machine you are on. In essence, a dev container is a preconfigured environment defined by a set of files usually using Docker to create isolated, consistent setups for development. Think of it as another computer running on your computer which includes everything you need to work on a specific project. This can include the right programming language, tools, libraries, dependencies, etc.

This is valuable because teams in the technology industry often work on complex projects that require a specific set of tools and dependencies. Without a dev container, each developer has to manually set up their environment on their machine. This could lead to errors, wasted time, and inconsistencies. However, with a dev container, everyone on the team works in an identical environment, reducing bugs caused by "it works on my machine" issues. Dev containers also simplify onboarding new team members and reduce wasted time since they can just start coding with a few lines of json in place.

### How are software project dependencies managed?
In most software projects, you will rely on external libraries to leverage work that has been done by others. Managing software dependencies is essential to ensure that your project has access to the correct versions of all of these libraries, avoiding compatibility issues. 

For Go, **Go Modules** manage dependencies. This system was introduced to standardize how external packages are fetched and versioned:
- **go.mod**: Lists your module’s name and its direct dependencies.
- **go.sum**: Locks down the exact versions of your dependencies to ensure consistent builds across machines.
We will see a simple example of dependency management in Go soon!

### Step 1. Add Development Container Configuration
1. In VS Code, open the `go-dev-container` directory. You can do this via: File > Open Folder.
    - For more convenience, follow [this.](https://muhammaddf.github.io/comp423-course-notes/tutorials/code-command/)
2. Install the Dev Containers extension for VS Code.
3. Create a `.devcontainer` directory in the root of your project with the following file inside of this "hidden" configuration directory: 

`.devcontainer/devcontainer.json`

``` json
{
  "name": "My Go Dev Container",
  "image": "mcr.microsoft.com/devcontainers/go:latest",
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": [
        "golang.Go"
      ]
    }
  },
  "postCreateCommand": ""
}
```
!!! Note 
    - `name`: A descriptive name for your container.
    - `image`: The Docker image we use. Microsoft maintains a variety of base images for popular languages, including Go.
    - `customizations`: Ensures that the recommended VS Code extensions (like the official Go extension) are installed for anyone who opens this project.
    - `postCreateCommand`: Commands to run right after the container is built. Since we only need a basic environment for "Hello World," we can leave it empty for now.

### Step 2: Reopen the Project in a VSCode Dev Container
To reopen the project in the container, press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> (or <kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> on Mac) and search for **Dev Containers: Reopen in Container**, then select it. This may take a few minutes, don't worry!

Once your dev container setup completes, close the current terminal tab and open a new terminal pane within VSCode. Run: `go version`. You should see your dev container running the latest version of Go. (As of January 2025, the latest stable Go version is go1.23.4)

## Part 3: Developing in Go

### Creating a New Module 
As we mentioned, Go uses **modules** to manage dependencies and organize your code. To initialize a new module named `hello_world` for your project, follow these steps:

(A) Create and switch to a new directory for your new module:
``` bash
mkdir hello_world
cd hello_world
```

(B) Initalize your module:
``` bash
go mod init hello_world
```
This creates a `go.mod` file in the hello_world directory. The file will look like this:
``` go 
module hello_world

go 1.xx

```
`module hello_world` declares this directory as a module named `hello_world`.

(C) Create a file named `main.go` in the `hello_world` directory:
``` bash
code main.go
```
!!! note
    The `code` command is not set up by default for VSCode. To set it up you can check out my colleague's tutorial [here.](https://muhammaddf.github.io/comp423-course-notes/tutorials/code-command/) Otherwise you can just manually do it in VSCode by right-clicking on the hello_world folder, creating a new file and naming it `main.go`

(D) Write your first program in Go!
``` go
package main

import "fmt"

func main() {
    fmt.Println("HELLO COMP423")
}
```

(E) You can now compile and run your code using the following command:
``` bash 
go run main.go
```

This command compiles your Go program and creates an executable in your directory.
!!! optional 
    If you want to compile and run your code manually, you can use the following commands:
    Use `go build` to compile your code and create an executable in your directory. Run the executable using `./hello_world` 
    to display your output.This is similar to C's `gcc [filename.c] -o [output_filename]` command that you might remember from COMP 211 used to compile a program and get an executable object file.

You should now see:
``` bat
HELLO COMP423
```

!!! success 
    You have built a development container for Go and successfully printed HELLO COMP423!


































