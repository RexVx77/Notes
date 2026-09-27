CH1: Continuous Integration

# Welcome to “Learn CI/CD”

In this course, we'll learn about Continuous Integration and Continuous Delivery (CI/CD) and how to use it to automate the process of building, testing, and deploying software.

We'll also deploy a production-ready web application to the internet using modern best practices!

## Forking a Repo

[Forking](https://docs.github.com/en/get-started/quickstart/fork-a-repo) is how you create your own copy of a repository on GitHub.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/nom5RLr-1280x648.png)

## Assignment

Open the base repository for this course [on GitHub here](https://github.com/bootdotdev/learn-cicd-starter).

Click the "fork" button to create a copy of the repo in your own GitHub account. When you're done, paste the URL of your _forked_ repo into the text box and submit.


# Run Notely

Clone down your forked repo onto your local machine:

```bash
git clone REPO_URL
```

Make sure it's _your_ forked repo! Cloning boot.dev's repo will cause errors!

We'll be using this repository throughout the course.

## Assignment

1. [ ] Take a look at the `README.md` in the root of the project for instructions on how to run the application locally.
2. [ ] Run it and open the hosted webpage in your browser.

**Run and submit** the CLI tests using the [Boot.dev CLI Tool](https://github.com/bootdotdev/bootdev) in another terminal.

# Branches

You might be primarily familiar with using Git in a single-user environment, pushing updates through a linear workflow. For example:

```bash
# working off of the "main" branch locally
git add .
git commit -m "undo Lane's giant mistake"
git push origin main
```

This works quite well for single developer side projects, but it doesn't work well when you're working within a team setting. What happens when a teammate makes changes to the same function you're working on and you both push directly to the `main` branch? What happens if you want a teammate to review your code before it's merged? What happens if you want to work on multiple features at once?

_Branches solve these problems._

## What Is a Branch?

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/ruLXQOz-1049x430.png)

A [branch](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-branches#about-branches) is (basically) a copy of your codebase. However, it's a _special_ kind of copy that makes merging new code changes from one branch to another simple and easy.

So far, you may have been only working on the `main` branch, but you can create as many branches as you want, and they're a great way to keep changes that are unrelated to each other isolated and contained.

In many teams, the `main` branch reflects the state of the codebase that's running in production. This means that the `main` branch should always be stable and ready to deploy. If you want to:

- Add a new feature
- Fix a bug
- Refactor some code

Then you should create a new branch to add those changes to. This allows you to work on those changes independently without affecting the `main` branch.

## Assignment

1. Check which branch you're currently on with `git branch`. You'll see a list of branches, with an asterisk next to the branch you're currently on:

```bash
* main
```

2. Create a new branch called `addtests`. I like to name my branches after the change I'm about to make, and in this case, we're about to add tests.

```bash
git switch -c addtests
```

3. When you create a new branch, it only exists locally. Push this new branch up to GitHub:

```bash
git push origin addtests
```

You can check on GitHub to make sure the branch exists.

![](https://docs.github.com/assets/cb-68964/images/help/repository/file-tree-view-branch-dropdown-expanded.png)

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

# Pull Requests

Pull requests are a way to propose changes to a codebase. They're a great way to collaborate with your team and ensure that your code is reviewed and passes tests before it's merged into the `main` branch for deployment and for other developers to work on.

## Assignment

1. [ ] While still on the `addtests` branch, make a change to the repo's `README.md` file by adding a line of text to the bottom: "MYNAME's version of Boot.dev's Notely app." Replace `MYNAME` with your name.
2. [ ] Push the new commit to the remote branch.
3. [ ] Open a pull request to "propose" merging your changes into `main`.
    1. [ ] Open your repo on GitHub
    2. [ ] Click on the "Pull requests" tab
    3. [ ] Click on the green "New pull request" button

**Make sure that:**

- The "base repository" is your forked repo, _not the original_ `/bootdotdev/learn-cicd-starter` repo.
- The base branch is `main`, and the compare branch is your branch, `addtests`.

You'll see a [diff](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-comparing-branches-in-pull-requests#about-pull-request-merges) of the changes between `main` and your branch. If you're happy with the changes, click on the green "Create pull request" button.

Do **not** merge the pull request! You'll need it open for upcoming lessons.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

# Continuous Integration

Continuous Integration (CI) is where developers regularly push code changes into a central repository, and by doing so, automated builds and tests are automatically run.

Those tests can include unit tests, integration tests, styling checks, linting checks, security checks or any other type of automated test. If _any_ of the tests fail, the build is considered "broken" and the developer is notified so they can fix it.

Click to hide video

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: code review

## Our Internal CI at Boot.dev

Here at Boot.dev we have CI tests that run each time a new pull request is opened. The reviewer doesn't need to manually check for formatting issues or run tests locally before approving the changes. It automates _part_ of the code review process.

_CI is all about automating as much of the testing and review process as possible._

## Assignment

1. [ ] While still on the `addtests` branch, create a new `.github` directory in the root of your repository
2. [ ] Create a new directory inside `.github` called `workflows`
3. [ ] Create a new workflow by adding a file called `ci.yml` inside `.github/workflows`:

Make sure you use the `.yml` extension. `.yaml` is valid, but we test for `.yml`.

GitHub Actions workflows are written in [YAML](https://en.wikipedia.org/wiki/YAML), and GitHub automatically checks for and runs workflows in the `.github/workflows` directory of your repository.

4. [ ] Open `ci.yml` in your editor and add the following:

```yaml
name: ci

on:
  pull_request:
    branches: [main]

jobs:
  tests:
    name: Tests
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v6

      - name: Set up Go
        uses: actions/setup-go@v6
        with:
          go-version: "1.27.1"

      - name: Force Failure
        run: (exit 1)
```

[By default](https://docs.github.com/en/actions/creating-actions/setting-exit-codes-for-actions), a step "succeeds" if it exits with a status code of `0` and "fails" if it exits with a status code other than `0`.

Every (good) CLI tool that I'm aware of follows the convention of exit code `0` = pass, anything else = fail. For example, if a test case fails, `go test` will exit with a status code of `1`.

_Don't worry, I'll explain each line of the workflow file in detail soon._

Let's make sure that our CI tests fail when our tests fail. The last step of our workflow file is:

```yaml
- name: Force Failure
  run: (exit 1)
```

This step always fails because it runs the command `exit 1`, which exits with a status code of `1`.

5. [ ] Commit and push your changes up to your remote branch. Then, on your pull requests page, you should see that the "tests" workflow runs and fails. Great! That means that our (very basic) CI is working.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

### CI/CD Workflow Security & Access Control

1. **Why Restrict Workflow Changes?**
   - Prevents unauthorized bypassing of critical tests and security scans.
   - Protects sensitive secrets and deployment infrastructure from accidental or malicious exposure.
   - Prevents runner resource abuse (e.g., cryptomining).

2. **Key Protection Strategies:**
   - **CODEOWNERS (`.github/CODEOWNERS`)**: Automatically mandates explicit review/approval from DevOps or Lead teams whenever `.github/workflows/` files are modified.
   - **Branch Protection Rules**: Requires PR reviews, passing status checks, and prevents direct commits to critical branches like `main`.
   - **Fork Protections (Open Source)**: Requires maintainer approval before running Actions on PRs from first-time or external contributors; automatically withholds repository secrets.
   - **Reusable Workflows**: Stores centralized, standardized CI/CD pipelines in a separate, restricted repository so feature repos only reference approved workflows rather than defining their own execution steps.
# Breakdown of GitHub Actions

Let's break down some of the concepts from the workflow file we created.

```yaml
name: ci

on:
  pull_request:
    branches: [main]

jobs:
  tests:
    name: Tests
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v6

      - name: Set up Go
        uses: actions/setup-go@v6
        with:
          go-version: "1.27.1"

      - name: Force Failure
        run: (exit 1)
```

## Workflows

A [workflow](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#workflows) is triggered when an [event](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#events) occurs in your GitHub repository. For example, we'll trigger our "tests" workflow when we open a pull request into the `main` branch.

In our case, the `ci.yml` file contains a single workflow called "ci", but we could have named it anything.

## Jobs

A workflow is made up of one or more [jobs](https://docs.github.com/en/actions/about-github-actions/understanding-github-actions#jobs). A job is a set of steps that run on the same [runner](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#runners), which is just a virtual machine that runs your job on GitHub's servers.

For now, we just have one job. You would typically have multiple jobs if you wanted to run your tests in parallel, or if you wanted to run your tests on multiple operating systems.

In our case, the `ci.yml` workflow contains a single job called "Tests".

## Steps

A job is made up of one or more steps. A step is a single task that can run commands, a script, or an action. For example, the steps of a job might include:

- Checking out the code
- Installing dependencies
- Running tests

In our case, the "Tests" job contains 3 steps:

1. Check out the code
2. Set up Go
3. Force failure of the CI job

## Assignment

Update the last step of the job to run `go version` instead of `(exit 1)`. That command should just print the current version of Go and exit with code `0`. Give the step a new name. Commit and push the changes, and your CI job should pass!

A workflow that triggers on pull requests will re-run when the branch to be merged is updated.

**Paste the URL of your GitHub repo into the box and run the GitHub checks**.

# GitHub Actions

Let's dissect this workflow file and understand what each line is doing.

```yaml
name: ci

on:
  pull_request:
    branches: [main]

jobs:
  tests:
    name: Tests
    runs-on: ubuntu-latest

    steps:
      - name: Check out code
        uses: actions/checkout@v6

      - name: Set up Go
        uses: actions/setup-go@v6
        with:
          go-version: "1.27.1"

      - name: Echo Go version
        run: go version
```

## Workflow Name

```yaml
name: ci
```

The first line simply assigns a human-readable name to the workflow.

## Triggering the Workflow

```yaml
on:
  pull_request:
    branches: [main]
```

The `on` key specifies when the workflow should run. In our case, we want to run the workflow when a pull request is opened to the `main` branch.

## Jobs

```yaml
jobs:
  tests:
    name: Tests
    runs-on: ubuntu-latest
```

The `jobs` key is a list of jobs that make up the workflow. In our case, we only have one job called `tests`.

Each job has a few pieces of metadata associated with it. The `name` key assigns a human-readable name to the job. The `runs-on` key specifies the type of [runner](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners) to use. In our case, we want to use the latest version of Ubuntu/Linux.

## Job Steps

```yaml
- name: Check out code
uses: actions/checkout@v6
```

Each job has a list of steps that make up the job. In our case, we have three steps. The first step checks out the code by using the pre-built [checkout](https://github.com/actions/checkout) action to clone the repository into the runner. You'll almost always want to include this step in your workflows. The `uses` key specifies the action to use, and the `with` key specifies the inputs to the action.

An [action](https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions#actions) is a reusable custom application that helps reduce the complexity of creating workflows. The [checkout](https://github.com/actions/checkout) action is a publicly available action that checks-out your repository so your workflow can access it.

```yaml
- name: Set up Go
uses: actions/setup-go@v6
with:
go-version: '1.27.1'
```

The second step sets up Go by using the pre-built [setup-go](https://github.com/actions/setup-go) action to configure a Go environment for use in workflows.

```yaml
- name: Echo Go version
  run: go version
```

The third step runs the `go version` command to print the version of Go installed in the runner. The `run` key specifies the command to run. The `run` key is used to run arbitrary command-line commands in the runner.

---

CH2: Tests

# Running Tests

Our current CI doesn't do anything interesting.

A good CI pipeline typically includes:

- Unit tests
- Integration tests
- Styling checks
- Linting checks
- Security checks
- Any other kind of automated test

If _any_ of the tests fail, the build is considered "broken" and the developer is notified (in our case by GitHub) so they can fix it.

## Assignment

The Notely repo has, _gasp_, zero unit tests!

1. [ ] Create `internal/auth/get_api_key_test.go`.
2. [ ] Add a couple unit tests for `GetAPIKey`.
    
    If you're still a little fuzzy on how to write unit tests in Go, I'd highly recommend Dave Cheney's [excellent blog post](https://dave.cheney.net/2019/05/07/prefer-table-driven-tests) on the subject.
    
3. [ ] Run them locally to make sure they work:
    
    ```bash
    go test ./...
    ```
    
    The `./...` tells Go to run all tests in the current directory and all subdirectories.

**Run and submit** the CLI tests from the **root of your repo**.

# Code Coverage

Code coverage is a measure of how much of your code is being tested. It's a controversial metric, but I'll try to provide a balanced take... granted I'm not without my own biases.

```
code_coverage = (lines_covered / total_lines) * 100
```

If you have `1000` lines of code in your project, and you have tests that cover the logic in `500` of those lines, then you have `50%` code coverage.

## Why Is Code Coverage Controversial?

It's quite possible to have 100% code coverage and still have bugs in your code. It's also possible to have 0% code coverage and have a bug-free application. Unit tests help us find bugs, and codify the expected behavior of individual units of code but they don't guarantee that there are no bugs.

My personal take is that it's really hard to say "20% is bad, 80% is good". I think some functions and methods are more important to have unit tests for than others. For example, I don't love the idea of [mock unit testing external systems, like databases](https://www.boot.dev/blog/backend/writing-good-unit-tests-dont-mock-database-connections/).

I think that's a better use case for [integration](https://circleci.com/blog/unit-testing-vs-integration-testing/) tests.

## What Does This Mean to You?

As a junior developer, you should know what code coverage _is_, and you should be amicable to the code coverage requirements of the company you work for. You'll certainly develop your own opinions (and should politely vocalize them) as you gain trust in your organization. It's important to be a team player and to be open minded to your team's processes, especially when you're new.

## Assignment

We won't fail our CI if there aren't enough unit tests, but we should at least print the coverage out to the console. Add the [-cover](https://pkg.go.dev/cmd/go#hdr-Testing_flags) flag to the `go test` command in your CI workflow, then commit and push your changes.

You should be able to inspect the logs of your latest workflow run in the GitHub UI (the "actions" tab) and see the code coverage report.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

# README Badge for Tests

One cool feature on GitHub is that you can add a [dynamic badge](https://docs.github.com/en/actions/monitoring-and-troubleshooting-workflows/adding-a-workflow-status-badge) to your `README.md` file that shows the status of your tests.

It's a great way to show off that your code is well-tested, and that the tests are passing without users having to go check the actions tab.

## Assignment

1. [ ] Add a badge to the top of your `README.md` file that shows the status of your tests. The syntax for the URL of the dynamically generated image is:

```markdown
https://github.com/<OWNER>/<REPOSITORY>/actions/workflows/<WORKFLOW_FILE>/badge.svg
```

Your `README.md` file is written in [markdown](https://www.markdownguide.org/cheat-sheet/), and the syntax for adding an image is:

```md
![alt text goes here](IMAGE_URL)
```

2. [ ] Commit and push your changes to GitHub, then merge them. Once your changes are merged into your `main` branch, you should see the badge appear on the main page of your repository in the rendered `README`.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

---

CH3: Formatting

# Formatting

Automated code formatting keeps your code consistent and readable. It's also a great way to avoid arguments about code style ([bikeshedding](https://en.wiktionary.org/wiki/bikeshedding)).

I almost never start a new project without enforcing automated code formatting.

For example, this is technically valid:

```go
func main() {
	fmt.Println("hello world!") }
```

But it's not formatted according to standard conventions. You would normally write it like this:

```go
func main() {
    fmt.Println("hello world!")
}
```

## go fmt

The `go fmt` command formats Go code. It's built into the Go toolchain, so you don't need to install anything additional to use it.

I have configured my editor to auto-format my code on save, so my code is always formatted. The default "Notely" project should already be formatted properly, so let's break it so that we can see how `go fmt` works.

## Assignment

1. [ ] While still on your `addtests` branch, edit one of the functions in the Notely codebase so that it's not formatted properly. For example, you could remove the whitespace between the function parentheses and body:

```go
func main() {
vs
func main(){
```

2. [ ] _Save the file without autoformatting it_ (you might need to get around auto-formatting if your editor does it automatically).
3. [ ] Run `go fmt ./...` in the root of your project. You should see the code get formatted properly.

**Run and submit** the CLI tests from the **root of your repo**.

# Check Formatting

Unfortunately (in my opinion) the `go fmt` command always exits with status code `0`. Luckily `go fmt` prints the names of all the files it fixes, so if we want to fail a CI check when a repo isn't formatted, the easiest way is to make sure that nothing is printed to stdout.

We can use the [test](https://en.wikipedia.org/wiki/Test_\(Unix\)) command to do so:

```bash
test -z $(go fmt ./...)
```

Let's break down how it works:

- `go fmt ./...`: Runs the go fmt tool on the current directory and all its subdirectories (that's what ./... stands for). `go fmt` returns the names of files that it has formatted. If no files need formatting, it will return an empty output.
- `$(go fmt ./...)`: The `$()` syntax is used for command substitution in bash. It runs the command inside the parentheses, and then replaces the `$()` in the command line with the output of that command.
- `test -z $(go fmt ./...)`: The `test` command is built into [bash](https://www.gnu.org/software/bash/). The `-z` option checks if the following argument is an empty string, returning `0` if it is, and `1` if it isn't.

## Assignment

1. [ ] Edit one of the functions in the Notely codebase so that it's not formatted properly. Save it and run:

```bash
test -z $(go fmt ./...)
```

2. [ ] Check the exit code:

```bash
echo $?
```

The `echo $?` command prints the exit code of the last command that was run. You should see that it prints `1`, indicating that the repo is _not_ formatted properly.

However, it should have also fixed the formatting!

3. [ ] Run this one more time:

```bash
test -z $(go fmt ./...)
echo $?
```

You should see that it prints `0`, indicating that the repo _is_ formatted properly.

**Run and submit** the CLI tests.

# Formatting CI

Now that we understand how to check for formatting issues, let's add a formatting check to the CI workflow.

## Check for Formatting Issues

```bash
test -z $(go fmt ./...)
```

## Assignment

1. [ ] Run the formatting check in the CI workflow under a separate "style" job. Give it the name "Style".

We _could_ simply add it as another _step_ within the same "tests" job, but I think it will be cool to run independent CI checks in parallel.

After all, tests and formatting aren't dependent on each other, so why _not_ run them in parallel and save some time? However, we will need to duplicate the "set up Go" and "check out code" steps in the new job because they are not shared between jobs.

2. [ ] Commit and push your changes to your remote branch.
3. [ ] Create another pull request from your branch into `main`. The CI workflow only runs the checks when a pull request is made to `main`, and since you merged your first pull request you'll need a new one.
4. [ ] Verify that both CI jobs pass.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

## Tips

If you have an open pull request and push new commits to the head branch (the one with the new changes) GitHub will automatically update the PR and rerun the actions.

---

CH4: Linting

# Linting

Code Formatting deals with the aesthetic appearance of the code. For example, it enforces things like whitespace, indentation, and line length.

On the other hand, [linting](https://en.wikipedia.org/wiki/Lint_%28software%29) has more to do with the analysis of code to detect functional issues. Linters provide warnings or errors for potentially problematic code.

## Staticcheck

Personally I use [staticcheck](https://staticcheck.io/docs/) for linting in Go. It has a lot of useful checks with sane defaults, and it's easy to configure. As far as I can tell it's essentially replaced [golint](https://github.com/golang/lint) as the most popular linter for Go.

## Install Staticcheck

To [install staticcheck](https://staticcheck.dev/docs/getting-started/), run:

```bash
go install honnef.co/go/tools/cmd/staticcheck@latest
```

If you're on Mac and already have [Homebrew](https://brew.sh/), you may run:

```bash
brew install staticcheck
```

## Run Staticcheck

To run `staticcheck` on the entire Notely codebase, run this from the root of the project:

```bash
staticcheck ./...
```

If you get a `command not found` error, your GOPATH may not be in your [PATH](https://en.wikipedia.org/wiki/PATH_%28variable%29). Run the following and use the appropriate config file name for your shell.

```bash
echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc
source ~/.bashrc
```

It _should_ run without errors because the project is already configured to pass all of the default checks. Let's make sure that's true by breaking something!

Add an unused function to the `main.go` file:

```go
func unused() {
    // this function does nothing
    // and is called nowhere
}
```

If you're using an editor with Go language server support (like Zed or VS Code with the Go extension), staticcheck should automatically detect the error and underline the function name. You can also run staticcheck manually from the CLI:

```bash
staticcheck ./...
```

You should see an error like this:

```bash
func unused is unused (U1000)
```

If so, great! Staticcheck is working properly.

**Run and submit** the CLI tests from the **root of your repo**.

# CI Linting

To get linting on our CI runner, we can't just call `staticcheck`: it's not installed on the runner!

Before any steps that _use_ the `staticcheck` command, you'll need to install it:

```yaml
- name: Install staticcheck
  run: go install honnef.co/go/tools/cmd/staticcheck@latest
```

## Assignment

Add a new _step_ to the same "style" job that currently only checks for `go fmt` issues. Run `staticcheck` after `go fmt` to make sure that formatting _and_ linting issues are caught by our pipeline.

Commit and push your changes with the `unused` function to your branch with the open pull request. You should see your CI workflow fail since `staticcheck` will catch the unused function.

Once you've verified that your CI workflow fails, remove the unused function and commit and push your changes again. Your CI workflow should now pass!

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

You can use the official [Staticcheck GitHub Action](https://github.com/marketplace/actions/staticcheck) instead.

# Linting Review

"Linting" can be such a vague and confusing term. I want to go over a few of the most common and useful checks that `staticcheck` lints for, just so you can see a few more examples.

If you're curious, you can see the full list of checks [here](https://staticcheck.io/docs/checks).

## Unused Variables (U1000)

This is the one we've been using so far. It checks for unused variables, functions, and types.

## Invalid Printf Call (SA5009)

This one is pretty self-explanatory. It checks for invalid `Printf` calls. For example, if you try to print a string with a `%d` format specifier, you'll get an error.

## “Defer” in Range Loops (SA9001)

Defers in range loops may not run when you expect them to, and in general you should just avoid them.

## Replace for Loops With Call to Copy (S1001)

Before:

```go
for i, x := range src {
    dst[i] = x
}
```

After:

```go
copy(dst, src)
```
---
CH5: Security

# Security Checks

Another common use case for continuous integration is static security checks. These are checks that can be run on your code to find potential security vulnerabilities.

There are _many_ products out there that do this sort of thing. One of the most popular open-source ones is [Gosec](https://github.com/securego/gosec).

## Install Gosec

```bash
go install github.com/securego/gosec/v2/cmd/gosec@latest
```

## Run Gosec

To run `gosec` on the entire Notely codebase, run this from the root of the project:

```bash
gosec ./...
```

You should see a few unhandled errors! That's okay. Don't fix them yet.

**Run and submit** the CLI tests from the **root of your repo**.

# CI Security

Like `staticcheck`, `gosec` is _not_ part of the Go toolchain. We'll need to install it in our remote runner before we can use it.

## Assignment

Security isn't a _style_ concern, so let's add these next steps after `go test` in the "Tests" job.

Add the install step.

```yaml
- name: Install gosec
  run: go install github.com/securego/gosec/v2/cmd/gosec@latest
```

Add another step to do a `gosec` check.

We want to be sure `gosec` works, so the first time you push the new `ci.yml` file to your PR branch, we are intentionally making the check fail. Don't fix the errors in the Notely project code yet.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

# Fix Security Issues

## Assignment

1. [ ] Update your code to fix the security issues found by `gosec`. Make sure you don't break any functionality when doing so.
2. [ ] Push the new code up to your PR to trigger a new CI run. It should pass now.

It's okay if a `.env` file doesn't exist, the server should simply print a warning and continue.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

## Tips

Make sure the code is bug free and the other checks will pass.

To fix the `gosec` issues, open the GitHub Actions logs for the failed run, find the `gosec` step, and read each finding.

# Security Review

Just because our codebase passes all of our tests, linters, and security checks doesn't mean it's "perfect" (or frankly even that it's "good").

These kinds of automated tests can help us eliminate ~80% of the most _obvious_ bugs, stylistic anti-patterns, and security vulnerabilities, but they can't catch everything. We still need to take care to write good, secure code.

CI is an amazing tool to automate a lot of the stuff you'd manually check anyway. However, just because the checks pass doesn't mean your code is perfect. You still need to use your brain!

---

CH6: Build
# Docker

We'll be using Docker to deploy Notely to Google Cloud Run. If you're not familiar with Docker, you should take our Learn Docker course [here first](https://boot.dev/courses/learn-docker).

Make sure you have Docker [installed](https://docs.docker.com/engine/install/) by running `docker --version` in your terminal. You should be on at least version `20.10.21`.

## Assignment

1. [ ] Run the `scripts/buildprod.sh` script found in the root of the Notely repository. This will produce a `notely` binary that's compiled for Linux, which is the OS our Docker image will run on.

```bash
./scripts/buildprod.sh
```

2. [ ] Build the Docker image locally:

```bash
docker build -t DOCKERHUB_NAMESPACE/notely:latest .
```

_`DOCKERHUB_NAMESPACE` should be replaced with your Docker Hub username._

3. [ ] Run the Docker image locally:

```bash
docker run -e PORT=8080 -p 8080:8080 DOCKERHUB_NAMESPACE/notely:latest
```

4. [ ] Open the app in your browser at `http://localhost:8080`. You should see the Notely app running locally!

**Run and submit** the CLI tests.

## Troubleshooting

If you get an architecture or exec format error, that's probably because your machine is different than the intended Docker Container.

**macOS**

1. [ ] Set `GOOS` to `linux` and `GOARCH` to `arm64` in the build script or set them when building the binary

```bash
GOOS=linux GOARCH=arm64 go build -o notely .
```

2. [ ] Set the `--platform` flag to `linux/arm64` in the `Dockerfile` or when running the `docker build` command.

```bash
docker build -t DOCKERHUB_NAMESPACE/notely:latest . --platform=linux/arm64
```

## Tips

Use `http`, not `https`!

Creating a user on the Notely site is not expected to work yet, we haven't set up the database!

# Continuous Deployment

Continuous Deployment (CD) is the process of automatically deploying code changes to a production environment after the code has been built and tested. Let's set up CD for Notely!

We'll be using GitHub Actions again, but we'll create a new workflow for CD: `cd.yml`

Click to hide video

Your browser does not support playing HTML5 video. You can instead. Here is a description of the content: continuous deployment

## Assignment

1. [ ] Create a new workflow `.github/workflows/cd.yml`
2. [ ] It should trigger on `push` into the `main` branch.

```yaml
on:
  push:
    branches: [main]
```

3. [ ] It should have a single job called `Deploy`
4. [ ] It should checkout the code
5. [ ] It should set up the Go toolchain
6. [ ] It should build the app using the `scripts/buildprod.sh` script

To be clear, the `ci` workflow runs when a pull request is _opened_, but the `cd` workflow runs when a pull request is _merged_ (or when code is pushed directly to the `main` branch).

Run the `cd` workflow and ensure it works by merging your PR into `main` (or pushing directly to `main`).

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

# Google Cloud Platform

GCP is one of the "big three" cloud providers, along with AWS and Azure. We're going to use GCP to host our Notely application!

Everything we do in this course falls under the [free tier](https://cloud.google.com/free) of GCP, at the time of writing. That said, you will need to provide a credit card to sign up, and you should be careful to not exceed the free tier and free trial limits if you don't want to be charged.

## Create a GCP Account

First, you'll need to create a GCP account. You can do that [here](https://cloud.google.com/).

## Create a Project

Once you've created an account, you'll need to [create a project](https://developers.google.com/workspace/guides/create-project).

Name the project `notely`.

One of my favorite aspects of GCP is how it groups resources by project. We'll keep everything for Notely in a single project, and when you're done with this course you can simply delete the project to clean everything up in one place.

## Create a Billing Account

Next, you'll need to [create a billing account](https://cloud.google.com/billing/docs/how-to/create-billing-account). This is where you'll provide your credit card information. You can find the billing section in the GCP console by clicking the hamburger menu in the top left, then "Billing".

Ensure your billing account is linked to your project, and you are able to see the billing information for your project in the GCP console.

# Google Cloud SDK

For some tasks, it makes sense to use the `gcloud` CLI instead of the GCP web console. For example, to run tasks from a GitHub Actions workflow, we'll need to use the `gcloud` CLI.

1. [ ] Install the `gcloud` CLI tool [here](https://cloud.google.com/sdk/docs/install).
2. [ ] [Initialize](https://cloud.google.com/sdk/docs/initializing) it by running `gcloud init` in your terminal. If you are using WSL, use `gcloud init --console-only` instead.
    - [ ] It will prompt you to login by opening a browser window. Login with the same account you used to create your GCP project.
    - [ ] Select your `notely` project.

## Assignment

Run the following command to verify your authenticated account and project settings:

```sh
gcloud config list
```

**Submit** the CLI tests from the **root of your repo**. There's no penalty on failure for this lesson.

## Troubleshooting Tips

You should already be authenticated after running `gcloud init`. If not, run:

```sh
gcloud auth login
```

If you are using WSL, you may need to restart your WSL instance after installing the gcloud CLI for it to be recognized in your terminal's PATH.

# Google Artifact Registry

We'll be using [Google Artifact Registry](https://cloud.google.com/artifact-registry/docs/overview) to store our Docker images. It's similar to Docker Hub, but it's private and hosted on GCP.

Whenever we create a new version of Notely, we'll build it into a new Docker image version and push that to Artifact Registry.

## Assignment

1. [ ] In the GCP console, search for and enable the `Cloud Build API`.
2. [ ] Open [Cloud Build Settings](https://console.cloud.google.com/cloud-build/settings) and enable the `Cloud Build Service Account` role for the default service account.
3. [ ] Within `Artifact Registry` in the GCP console, enable the Artifact Registry API.
4. [ ] Click `Create Repository`:
    - [ ] Name: `notely-ar-repo`
    - [ ] Format: `Docker`
    - [ ] Mode: `Standard`
    - [ ] Location type: `Region`
    - [ ] Region for deployment: `us-central1`
    - [ ] Leave "Google-managed encryption key" selected
    - [ ] Disable vulnerability scanning if the option appears; we don't need paid scans for this course

The image hosting region from earlier, and service deployment region we are targeting now, may not necessarily be the same region. Cloud providers provide flexibility with [availability zones](https://cloud.google.com/compute/docs/regions-zones), so that engineers can pick and choose the most optimal regions for your system.

5. [ ] Build and push the Docker image to Artifact Registry:

```bash
gcloud builds submit --tag us-central1-docker.pkg.dev/PROJECT_ID/notely-ar-repo/notely:latest .
```

Replace `PROJECT_ID` with the output of `gcloud config get-value project`.

**Run and submit** the CLI tests from the **root of your repo**.

# Automate Builds

Now that we've built the Docker image locally, let's build it automatically on every push to the `main` branch.

## Assignment

Use the [setup-gcloud](https://github.com/google-github-actions/setup-gcloud) action to authenticate with GCP.

I recommend using the simple [service account key JSON](https://github.com/google-github-actions/setup-gcloud#service-account-key-json) setup.

### Creating a Service Account

1. [ ] Go to the [IAM & Admin Service Accounts](https://console.cloud.google.com/iam-admin/serviceaccounts) section of the GCP console.
2. [ ] Create a service account and name it "Cloud Run Deployer" with these permissions:
    - [ ] `Cloud Build Editor`
    - [ ] `Cloud Build Service Account`
    - [ ] `Cloud Run Admin`
    - [ ] `Service Account User`
    - [ ] `Viewer`
3. [ ] Create a JSON key for that service account and download it to your computer.

### Add the Key As a Secret in GitHub Actions

4. [ ] Go to your GitHub Repo > Repository Settings > Secrets and variables > Actions > New repository secret (**not** "environment secret" - use a repository secret)
    - [ ] Name: `GCP_CREDENTIALS`
    - [ ] Secret: Paste the entire JSON key from the file you downloaded from GCP
5. [ ] Save the secret

### Update Your GitHub Action Workflow

6. [ ] After the `buildprod` script runs, add the [setup-gcloud](https://github.com/google-github-actions/setup-gcloud) steps to setup the `gcloud` CLI and authenticate with GCP.
7. [ ] Finally, add a step to build the Docker image and push it to Google Artifact Registry.

```bash
gcloud builds submit --tag us-central1-docker.pkg.dev/PROJECT_ID/notely-ar-repo/notely:latest .
```

Replace `PROJECT_ID` with the output of `gcloud config get-value project`.

8. [ ] Commit and push your changes to GitHub. You should see the GitHub Action run and successfully build and push the Docker image to Google Artifact Registry.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

---

CH7: Deploy

# Google Cloud Run

Cloud Run is a [serverless](https://www.cloudflare.com/learning/serverless/what-is-serverless/) container hosting service. It's a great fit for Notely because it has a generous free tier, scales automatically, and automagically configures pesky infrastructure like:

- Load balancing
- DNS
- HTTPS

_In a nutshell, we give Cloud Run a Docker image, and it runs it for us._

## Cloud Run Services

Cloud run has 2 types of applications:

- Services
- Jobs

A "service" is a Cloud Run application that listens and responds to web requests, as opposed to a "job" which is simply a task that runs to completion.

Notely makes sense as a service because it's a web application that needs to respond to web requests. It needs to serve the frontend, and it needs to respond to API requests.

## Assignment

For now, let's create a test service. Once we're comfortable with the process, we'll deploy Notely.

1. [ ] Navigate to the [Cloud Run](https://console.cloud.google.com/run) section of the GCP console.
2. [ ] Click "Deploy Container" and select "Service"
3. [ ] Select "Deploy one revision from an existing container image"
4. [ ] Container image URL: `bootdotdev/getting-started`
5. [ ] Service name: `test`
6. [ ] Region: `us-central1`
7. [ ] Under Authentication, choose "Allow public access". This will allow anyone to access the webpage the service serves without having to log in to GCP
8. [ ] Service scaling, set maximum number of instances to `4`.
9. [ ] Ingress control: "All" checked (allow direct access to the service)
10. [ ] Go to "Container(s), Volumes, Networking, Security". Under "Container(s)" change the "Container port" from `8080` to `80`.
11. [ ] Create the service
12. [ ] Wait for the service to deploy
13. [ ] Click the service's URL to see the webpage it serves

**Run and submit** the CLI tests and **use the URL of your service** (e.g. `bootdev config base_url https://test-vo4kpyh36a-uc.a.run.app/`).

# Cloud Run Review

What just happened was actually pretty amazing. A sysadmin from the early 2000s would be blown away by how easy it is to deploy a web application today. You started with a Docker Image that holds a lightweight, portable, and self-contained version of a web application, and in one click you deployed it to the public internet, complete with:

- Load balancing
- DNS (albeit a long, ugly URL)
- Auto-scaling (As more and more HTTP requests come in, Cloud Run will automatically spin up more instances of your app to handle the load)
- HTTPS

It's important to understand that many companies have a more complex setup and do a lot of that stuff manually. Levels of complexity vary dramatically in CI/CD automation from one company to the next, just like levels of complexity vary dramatically in applications from one company to the next.

Just to give you a taste of the variety of CI/CD flavors, I want you to know that some companies isolate CI from CD into two separate systems, e.g. Jenkins for CI vs Rundeck for CD. On the flip side, you will also find companies that bundle CI/CD together into a single job that builds and deploys the application. If there's a different way to do CI/CD, there's a company out there _somewhere_ prototyping it. Now that's enterprise coding!

Regardless, the principle is the same everywhere. At its core, CI/CD enables us so that:

- When PRs are opened, run static analysis and tests
- When PRs are merged, build and deploy the app automatically

That "build and deploy the app automatically" can be 10 lines of `yaml` or it can be thousands of lines of custom `bash` scripts. It all depends on the complexity of the app and the needs of the company.

## What Makes a “Good” CI/CD Pipeline?

- **Deterministic builds.** The same code should always produce the same build.
- **Fast builds.** The faster the better. This makes getting bug fixes and new features out to users faster.
- **Portable.** This is why I love when the majority of a CI/CD pipeline is just `bash` scripts. It's easy to run locally, and it's easy to run on any CI/CD platform.
- **Fully automated.** The fewer manual steps, the better. It's really annoying to manually run database migrations and click buttons. It's also error-prone.

# Delete the Test

It's _very_ bad practice to leave unused resources lying around in your cloud provider. It can be:

- Expensive
- Confusing
- A security risk

## Assignment

Delete the "Getting Started" test service you just created.

# Cloud Run Custom

Let's run Notely on Google Cloud Run!

Create a new service, but this time instead of using a sample container, use our new image hosted in Artifact Registry that we built previously.

## Assignment

1. [ ] Go to the [Cloud Run](https://console.cloud.google.com/run) section of the GCP console.
2. [ ] Go to "Services" and click "Deploy Container".
    - [ ] Choose `Deploy one revision from an existing container image`
    - [ ] Container Image URL: Select your container image from the Artifact Registry
    - [ ] Name: `notely`
    - [ ] Region: select `us-central1`
    - [ ] Authentication: check `Allow public access`.
    - [ ] Billing: check `Request-based`
    - [ ] Service scaling: check `Auto scaling` and set the Maximum number of instances to `4`.
    - [ ] Ingress: check `All` (to allow direct access to the service)
3. [ ] Create the service, and wait for it to deploy
4. [ ] Click the service's URL to see it in action!

**Run and submit** the CLI tests and **use the URL of your service** (e.g. `bootdev config base_url https://test-vo4kpyh36a-uc.a.run.app/`).

# Cloud Run Updates

Now that we have a service configured and running, let's update our GitHub actions workflow to automatically deploy changes to the app when we push to the `main` branch.

## Assignment

1. [ ] Add a new step to the `deploy` job in the GitHub actions workflow to deploy the app to Cloud Run. Because we want to allow unauthenticated access to the app, we'll also need to add a new security setting to the Cloud Run service. I recommend simply adding this to the same step in CD:

```yaml
- name: Deploy to Cloud Run
  run: gcloud run deploy notely --image REGION-docker.pkg.dev/PROJECT_ID/REPO_NAME/IMAGE:TAG --region REGION --allow-unauthenticated --project PROJECT_ID --max-instances=4
```

2. [ ] Target the `us-central1` region for service deployment and the latest image.

## Make a Change and Push to GitHub

3. [ ] Change the `/static/index.html` file so that the `h1` tag says "Welcome to Notely" instead of just "Notely".
4. [ ] Commit and push your changes to GitHub. You should see the GitHub Action run and successfully deploy the new version of the app to Cloud Run.
5. [ ] Open the URL in your browser and you should see the new version of the app.

**Submit** the CLI tests and **use the URL of your service** (e.g. `bootdev config base_url https://test-vo4kpyh36a-uc.a.run.app/`). There's no penalty on failure for this lesson.

---

CH8: Databases

# Turso

Turso is a cloud provider that specializes in hosting serverless [SQLite-like](https://www.sqlite.org/) databases (technically it's their fork, [libsql](https://github.com/tursodatabase/libsql)). It's a great fit for Notely because it has a _very_ generous free tier, and it's easy to use.

## Assignment

### Create an Account

- Create an account [here](https://turso.tech/)
- Complete the onboarding process
- Create your first database with the name: `notely-db`

### Install and Configure the Turso CLI

Here's the full [quickstart guide](https://docs.turso.tech/quickstart) in the Turso docs, but in a nutshell:

1. [ ] Install the CLI:

```sh
# macOS
brew install tursodatabase/tap/turso

# Linux / WSL
curl -sSfL https://get.tur.so/install.sh | bash
```

Note that you may need to restart your shell to make sure the changes have taken effect.

2. [ ] Login

```sh
turso auth login
```

3. [ ] Show your databases

```sh
turso db list
```

4. [ ] Connect to your database

```sh
turso db shell notely-db
```

5. [ ] Run a simple SQL query

```sql
SELECT sqlite_version();
```

6. [ ] Create a test table:

```sql
CREATE TABLE test (
  id INT,
  name TEXT
);
```

7. [ ] Show the table:

```sql
SELECT name FROM sqlite_master WHERE type='table' ORDER BY name;
```

8. [ ] Delete the table:

```sql
DROP TABLE test;
```

9. [ ] Exit the shell with Ctrl+C.

This SQL shell is a great way to manually interact with your database for debugging purposes, but our server will of course interact with it programmatically.

**Run and submit** the CLI tests from the **root of your repo**.

# Migrate Automatically

There are many ways that teams handle database migrations in continuous deployment environments.

Some teams prefer to run migrations as part of the deployment process, others run them manually, and some have more complete systems featuring downward migrations. Downward migrations provide one more tool for recovery in case of a problem with a recent database schema change. For simplicity, this course runs migrations via our GitHub Actions deployment script.

We'll simply migrate the DB to the latest version every time we deploy. That way, if the code we're deploying requires a new schema, we'll always have it. The only time this can be a problem is if a migration makes backward-incompatible changes to the schema (like dropping a table). If the currently running application needs a table that we drop, it will stop working until the new code is deployed.

_That's bad, downtime is bad._

To avoid those scenarios, we could simply roll out the _code_ that stops relying on the hypothetical table first, then in the _next_ deployment remove the table in a migration.

## Assignment

1. [ ] Add a `DATABASE_URL` secret to your GitHub repo's Settings -> Secrets and variables -> Actions. Paste your full `DATABASE_URL` from your `.env` (including the `?authToken=...`). The migration will fail if you wrap it in quotes.

Add a new section to the `cd.yml` deploy job, that grabs the secret and provides it to the rest of the job environment.

```yaml
jobs:
  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    # This part
    env:
      DATABASE_URL: ${{ secrets.DATABASE_URL }}
```

2. [ ] Update the `cd` workflow in your GitHub Actions `cd.yml` file to run migrations using the `migrateup.sh` script _before_ deploying the application.

I'd recommend running it _after_ the Docker image is built, but _before_ the deployment of the image. That accomplishes two things:

1. We won't run the migration if there is a problem building the image.
2. The migration will be live before the new code is deployed.

Remember to install `goose` in the runner, right after you set up Go (it's installed with `go install`).

Finally, you can run `git diff` / `git diff HEAD` to check for any sensitive credentials like database connection strings, that may have slipped into your source code. We've taken the liberty of `.gitignore`'ing the `.env` file already, but `git diff` can point out credentials mistakes even before you commit code changes.

Now commit the code. Push it to GitHub, then make sure the workflow runs as expected.

_Paste the URL of your GitHub repo into the box and run the GitHub checks._

# Using the DB

Migrations are done! Now let's configure our Cloud Run app to _use_ the database.

## Assignment

1. [ ] Go to the [secrets manager](https://console.cloud.google.com/security/secret-manager) in GCP (enable the API if you have to) and create a new secret:
    - Name: `notely_db_password`
    - Paste your database URL into the value field
2. [ ] Go back to the [Cloud Run](https://console.cloud.google.com/run) page and select your app. Click "Edit & Deploy New Revision" and then under "Container(s)" -> "Variables & Secrets" -> "Reference A Secret". Supply the name `DATABASE_URL`. Select the `notely_db_password` secret. Select `latest` version. Select `Done`.
3. [ ] Select `Deploy`. (We expect this deployment to fail with a permission error.)
4. [ ] Back in the Google Cloud [IAM & Admin](https://console.cloud.google.com/iam-admin/iam) dashboard, edit the `Cloud Run Deployer` service account and assign it a new role, `Secret Manager Secret Accessor`.
5. [ ] Return to the Cloud Run notely service and edit the service once more. Select the `Security` tab. We want to use the `Cloud Run Deployer` service account.
6. [ ] Select `Deploy` again. (This deployment should work better.)

Let's test the application deployment.

You'll know the app is fully operational when:

- You can create new users
- You can create new notes
- Refreshing the page indicates that you are "logged in"
- You can view notes that you create

**Run and submit** the CLI tests and **use the URL of your service** (e.g. `bootdev config base_url https://test-vo4kpyh36a-uc.a.run.app/`).

# CI/CD Review

Congratulations on deploying a full-stack web application to the public internet!

## Cleanup

When you're done with Notely:

1. [ ] [Shut down](https://console.cloud.google.com/iam-admin/settings) your GCP `notely` project to delete its course resources.
2. [ ] Run [`turso db destroy`](https://docs.turso.tech/cli/db/destroy) `notely-db` to delete your Turso database.

## Recap of Your Accomplishments

- You set up a continuous integration pipeline with GitHub Actions that ensures new PRs pass certain checks before they are merged to `main`:
    - Unit tests pass
    - Formatting checks pass
    - Linting checks pass
    - Security checks pass
- You configured a cloud-based SQLite database hosted on Turso
- You set up a continuous deployment pipeline with GitHub Actions that does the following whenever changes are merged into `main`:
    - Builds a new server binary
    - Builds a new Docker image for the server
    - Pushes the Docker image to the Google Artifact Registry
    - Deploys a new Cloud Run revision with the new image and serves the app to the public internet

Pat yourself on the back! That's a pretty robust setup for our simple CRUD app.

## Some Things to Keep in Mind

- Google Cloud Platform (GCP) is just one of the 3 major cloud providers. AWS and Azure are also popular choices. In many ways, their offerings are similar, but sometimes the differences matter.
- Google Cloud Run handles a lot of complexity for you. Managing DNS, SSL, load balancing, and auto-scaling are all things that many companies do manually, so those are still useful skills to have, but are outside the scope of this course.
- Turso is a fully-managed third-party database host. There are _many_ options out there for databases and database hosting that are worth learning about, but again, outside the scope of this course.
- Essentially every technology/product we used in this course has viable alternatives. You don't need to know how to use all of them before your first job, but you should understand _some_ of them.