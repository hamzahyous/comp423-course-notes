# Golang DevContainer 
* Primary author: [Hamzah Yousuf](https://github.com/hamzahyous)
* Reviewer: [Katun Li](https://github.com/katunli)

## i. Introduction
This is a tutorial on how to set up a DevContainer in VSCode spesific to the Go programming Language (Golang). Some of the information provided is thanks to the [Mkdocs tutorial](https://comp423-25s.github.io/resources/MkDocs/tutorial/) completed prior to this assignment, and will be cited throughout as such. 
To begin this process, there are a few [**prerequisite softwares**](https://comp423-25s.github.io/resources/MkDocs/tutorial/) you must have installed, and a list is provided below.

- A Github account: If you do not have one, [click here](https://github.com/)
- Git Installed: Check installation in your terminal with `git --version`. If none is given, [click here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- Visual Studio (VS) Code: If you do not have it, [install here](https://code.visualstudio.com/)
- Docker: Required to run the dev container. If you do not have it, [click here](https://www.docker.com/products/docker-desktop/)
- A basic grasp of a command line interface (CLI)

## ii. Git Repository Setup
An essential component of this tutorial is to create and setup a Git repository for your project. We begin by creating a local directory and initialzing Git. The outline of the steps is inspired by the prior [MkDocs tutorial](https://comp423-25s.github.io/resources/MkDocs/tutorial/).

### Part 1 - Initializing git in a Local Directory

- Open your terminal or command prompt. We will begin by creating the directory, then switching into it.
```bash
mkdir go-tutorial
cd go-tutorial
```
- In the current directory, we can initialize a Git repository as done below:
```bash
git init
```
- Then we create a README file:
```bash
echo "# Go Programming Language tutorial" > README.md
git add README.md
git commit -m "Initial commit with README"
```

### Part 2 - Creating a remote repository on GitHub

- In your GitHub account, Click on your profile picture, navigate to *Your Repositories*, and then click *New* to create a new repository.
- Here is a template for the details to enter on that page:
    - **Repository Name:** `go-tutorial`
    - **Description:** "Creating a DevContainer spesific to Golang along with a 'Hello COMP423' program."
    - **Visibility**: Public
    - **Note:** When initializing, do not do so with a README, .gitignore, or license.
    - Click **Create Repository**.

### Part 3 - Linking your repository to GitHub
The third and final part of the repository setup is to link the GitHub repository as remote. This is done back in the terminal or command prompt.

- You can add the repository like so, replacing `<your-username>` with your GitHub username:
```bash
git remote add origin https://github.com/<your-username>/go-tutorial.git
```

!!! Note

    Check your default branch name with the subcommand `git branch`. The default branch is usually main, but older versions of git had the default branch named `master`. If yours is not `main`, you should make it `main` with the following command: `git branch -M main`.

- Now you are ready to push your local commits to the GitHub repository. 
``` bash
git push --set-upstream origin main
```

## iii. Setting up the DevContainer


Setting up a Development (Dev) Container is important because it provides a standardized environment for software development. Preconfiguring your environment this way ensures that it is replicable across different machines, striving for consistency in setup. This essentially equips all contributors with the same "development toolkit". <br>

Most projects use external libraries or packages, speeding up the process by making use of the resources provided by others. For this tutorial in go, the primary dependancy is `go mod`, which handles dependencies through two files, `go.mod` and `go.sum`.

`go.mod` is responsible for automatically tracking the dependencies of the project. `go.sum` is responsible for ensuring that every developer gets the same exact dependencies. You should regularly commit these files to your repository to make sure every collaborator has the same environment. Running the command `go mod tidy` reads the `go.mod` file, ensures the same dependencies are present, and then validates that against the `go.sum` file. 

Additionally, we will have a `devcontainer.json` file that defines a consistent development environment in which our code will run. This is done using a Docker image, which is a package containing everything necessasary to run our software, including code, runtime, libraries, and more. Below we detail the process.

### Part 1 - Add Development Container Configuration

1. Open the `go-tutorial` directory in VSCode. This is done by clicking File then Open Folder. 
2. In VSCode, click extensions and install the **Dev Containers** extension.
3. At the root of your project, create a `.devcontainer` directory (considered hidden with the .) with the following file inside: ```.devcontainer/devcontainer.json```. 

!!! Info

    The ```devcontainer.json``` file defines the configuration of our development environment. 

``` json
{
  "name": "Go Tutorial",
  "image": "mcr.microsoft.com/vscode/devcontainers/go:1.20",
  "customizations": {
    "vscode": {
      "settings": {
        "go.gopath": "/go",
        "go.formatTool": "gofmt"
      },
      "extensions": ["golang.go"]
    }
  },
  "postCreateCommand": "go mod tidy"
}
```

This configuration specifies the following:

- `name`: Choose a relevant name for the Dev Container. 
- `image`: The Docker image to use, for us it is the latest version of a Golang environment. 
- `customizations`: Settings specific to Go are added, setting the `GOPATH` directory for the containerized environment. `"go.formatTool": "gofmt"` specifies the tool for formatting Go code. 
- `postCreateCommand`: A command that is run after the container is created. For us, it is `go mod tidy`, which fetches and organizes Go dependencies listed in `go.mod`.

### Part 2 - Add Golang Dependancy Configuration 

`go.mod` is the Go specific file for listing the project's required dependencies, along with their versions. Go is unique because the `go.mod` file is automatically generated and managed with Go commands. To create it, run `go mod init github.com/<your-username>/go-tutorial` in the VS Code terminal. <br>

Managing these dependencies is done primarily with two commands, `go get` and `go mod tidy`. `go mod` specifies the subcommand mod, which is used generally for managing depdencies and initializing project structure. 

- `go get`: This automatically updates the `go.mod` and `go.sum` files. 
    - Format: `go get <module-path>@<version>`
- `go mod tidy`: Adds missing dependencies required by your project. Also removes unused dependencies from `go.mod` and `go.sum`.

In our case, our Hello COMP423 program has no external dependencies, you can skip over the part on managing dependencies and just run `go mod init github.com/<your-username>/go-tutorial`.

### Part 3 - Reopening the project in a VSCode DevContainer
Now that we've created the project, it's time to actually open it within a VSCode Dev Container. To reopen the project in the container, press `Ctrl+Shift+P` or (`Cmd+Shift+P` on Mac). This will open a window to type, where you should write "Dev Containers: Reopen in Container," selecting the option that pops up. This process may take a few minutes as you are downloading the image. 

Once the dev setup is completed, you can hit "Kill Terminal" on your VSCode terminal, and open a new one, running `go version` which should show you what version of Golang your container is running. As of now this should be version 1.20.

## iv. Hello COMP423

Congratulations, you've made it to the last section. A brief recap of where we are at: 

```
File Structure:

go-tutorial/
├── .devcontainer/
│   └── devcontainer.json
├── go.mod
└── main.go (we haven't made this yet)

```

### Part 1 - Creating main.go
Currently, we need to create a file to write our "Hello COMP423" program. At the root of the project, you need to create a file called `main.go`. If you haven't written in Go before, you can learn more about the syntax [here](https://medium.com/@manus.can/learn-golang-basic-syntax-in-10-minutes-48608a315896). However, for simplicity's sake, there will be code provided below to walk you through it. 

```go
package main
import "fmt"
func main() {
    fmt.Println("Hello COMP423")
}
```
!!! Info

    - `package main`: Defines the main package. Where execution begins. 
    - `import "fmt"`: Imports the `fmt` (format) package for formatted I/O.
    - `func main() {...}`: Entry point for our Go program. 

Now that we've written our simple program, we can move onto running it. 

### Part 2 - Running main.go

For this portion, a couple subcommands are useful to know to display to standard output. 


- `go build` : Compiles the program into a standalone executable (binary). Does __NOT__ execute the compiled program. Produces a binary file (`main`) which you can run seperately.
```bash
go build <file>.go
```

- `go run` : Compiles and executes the program in a single step. Does __NOT__ create a permanent binary file. Automatic execution of the compiled program. Good for testing or debugging your program.
```bash
go run <file>.go
```

First we can try `go run main.go`. This should print the following to standard output:

```bash
Hello COMP423
```

If we wanted to manually compile the executable and then run it we can type:
```bash
go build main.go. 
./main
```

This concludes the tutorial for setting up a DevContainer specific to Golang, then writing and running a simple "Hello COMP423" program. If you've made it to here, congratulations!