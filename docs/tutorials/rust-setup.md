# Setting up a dev container for Rust

* Primary author: [Lingxiao Han](https://github.com/Lingxiao-Han)
* Reviewer: [Wai Phyo Hein](https://github.com/waiphyo04)

## Part 1. Prerequisites
Please ensure you have all the below prerequisites before starting this tutorial.

1. A GitHub Account: Setting up here [Github](https://github.com)
2. Git Installed: Guide for [Mac](https://git-scm.com/downloads/mac) and [Windows](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
3. Visual Studio Code Installed: Install here: [VS Code](https://code.visualstudio.com/download)
4. Docker Installed: Install here: [Docker](https://www.docker.com/get-started/)
5. Command-line Basics: Here are cheatsheets [Mac](https://phoenixnap.com/kb/wp-content/uploads/2023/05/mac-terminal-commands-cheat-sheet-pdf.pdf) or [Windows](https://phoenixnap.com/kb/wp-content/uploads/2023/01/windows-cmd-commands-cheat-sheet-pdf.pdf)

## Part 2. Setting Up Repository and Git
### Create a Local Repository and Initialize Git
(A) Open your terminal or command prompt.

(B) Navigate to the repository that you want to put this project in.

(C) Using following command to create a new repository for your rust project and cd into it.
```bash
# Create a new repository called rust-language-tutorial
mkdir rust-language-tutorial
# Navigate into this repository
cd rust-language-tutorial
```
(D) Initialize a new Git repository 
```bash
git init
```
(D) Create a README file:
```bash
echo "# rust Language Project" > README.md
git add README.md
git commit -m "Initial commit with README"
```
### Setup a remote repository on Github
1. Login your GitHub account and open [create a new repository page.](https://github.com/new)
2. Fill in the detail as follow:
    - **Repository Name:** `comp423-rust-project`
    - **Description:** "A basic coding project in rust language"
    - **Visibility:** Public
3. Do not initialize the repository with a README, .gitignore, or license.
4. Click **Create Repository**.

### Link your Local Repository to GitHub
1. Add a GitHub repo as a remote:
```bash
git remote add origin https://github.com/<REPLACE_WITH_YOUR_USERNAME>/comp423-rust-project
```
remember to replace "<REPLACE_WITH_YOUR_USERNAME\>" with your actual GitHub username.
2. check your default branch's name by
```bash
git branch
```
if the name is not main, please run this command to rename it
```bash
git branch -M main
```
3. Push your local commits to GitHub repo
```bash
git push --set-upstream origin main
```
!!! note

    `git push --set-upstream origin main`: This command pushes the main branch to the remote repository origin. The `--set-upstream` flag sets up the main branch to track the remote branch, meaning future pushes and pulls can be done without specifying the branch name and just writing git push origin when working on your local main branch. This long flag has a corresponding `-u` short flag.

## Part 3. Setting Up the Development Environment
1. In VS Code, open the comp423-course-notes directory. You can do this via: File > Open Folder.
2. Install the **Dev Containers** extension for VS Code.
3. Create a `.devcontainer` directory in the root of your project with the following file inside of this "hidden" configuration directory:

`.devcontainer/devcontainer.json`

The devcontainer.json file defines the configuration for your development environment. Here, we're specifying the following:

- **name:** A descriptive name for your dev container
- **image:** The Docker image to use, in this case, the latest version of a Python environment. [You can find many Docker image in this Microsoft page](https://hub.docker.com/r/microsoft/vscode-devcontainers)
- **customizations**:  
  - **vscode**: These settings enhance the VS Code experience within the container.  
    - **settings**: Customize VS Code editor settings for this project.
    - **extensions**: This indicates any VS Code extension that we want to include in this container. In this case, we includes the `rust-lang.rust-analyzer` extension, which provides advanced Rust language support.
- **postCreateCommand:** A command to run after the container is created. `cargo new Hello-World --bin --vcs none` create a new binary project called `Hello-World` in Rust without automatically initializing a Git repository.
```json
{
  "name": "comp423-rust-project",
  "image": "mcr.microsoft.com/devcontainers/rust:latest",
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": ["rust-lang.rust-analyzer"]
    }
  },
  "postCreateCommand": "cargo new Hello-World --bin --vcs none"
}
```
!!! note

    `cargo new Hello-World --bin --vcs none`: This command create a new binary project named `Hello-World`. `new` is a subcommand of cargo which creats a new Rust project in a new directory. `Hello-World` is the name of the directory. `--bin` is a flag specify it is a binary project, which is an `excutable program`. Without `--bin`, `cargo new` will instead create a new `library project`, which is a library for other programs. `-vcs none` is a flag disable version control initialization. We dont want a new git repo because we already have one.

Then, reopen the project in the container by pressing `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac), typing `Dev Containers: Reopen in Container,` and selecting the option. This may take a few minutes while the image is downloaded and the requirements are installed.

Once your dev container setup completes, close the current terminal tab (trash can), open a new terminal pane within VSCode, and try running `rustc --version` to see your dev container is running a recent version of Rust (`rustc 1.83.0 (90b35a623 2024-11-26)`) (As of Jan.24 2025).

## Part 4. Run Your First Rust Project
(A) You can now see an repository called `Hello-World` in your working folder. This is your Rust project. Open a terminal in VS Code and using following command to `cd` into the project.
```bash
cd Hello-World
```
(B) Find `Hello-World/src/main.rs` file, you can see a finished basic hello world program in Rust. Now, we will write our own output as follow:
```Rust
fn main() {
    println!("Hello COMP423");
}
```
(C) Then, like `gcc` command in C, we can build Rust program into an executable file. Run the following command to build our cargo project.
```bash
cargo build
```
!!! note "Difference between gcc and cargo build"
    In simple words, gcc is a simpely a compiler but cargo build is a build tool. Think of cargo as a compiler + project manager + dependency handler in one package, while gcc is just the raw compiler.
(D) You can now find the build project at `/target/debug/Hello-World`. Run following command to execute your first Rush program!
```bash
./target/debug/Hello-World
```
You will see output `Hello COMP423` in your terminal.
(E) Then we are going to explore a different way of running our Rust program. First, we clean our build.
```bash
cargo clean
```
(F) This time, we are going to use `cargo run` command to run our project.
```bash
cargo run
```
You will see output `Hello COMP423` in your terminal.
!!! note "Difference between cargo build and cargo run"
    `cargo run` combines building and executing into one command. `cargo build` is only building. Use `cargo run` if you want to actively developing and testing your program. Use `cargo build` when you are preparing your program for deployment or further steps.

Yeah! you finished your first rust program!

## Part 5. Push to GitHub
(A) Add and commit your changes
```bash
git add .
git commit -m "Finished rust Hello World program"
```
(2) Push the changes to GitHub:
```bash
git push origin main
```