# Documentation for Git Project

## **Initializing a Repository and Making Commits**

### Initializing a Git Repository

- Open a terminal on computer,eg git bash,
- create working folder or directory; `mkdir Devops`
- change or move into working folder; `cd Devops`
- run `git init`

![Alt text](<images/init git repo.png>)

### Making Commit

- Create a file `touch index.txt`
- Write a sentence inside the text file and save
- Add your changes to git staging area using the command `git add .`
- To commit changes to git, run the command `git commit -m "initail commit"`

![Alt text](<images/git commit.png>)

## **Working with Branches**

1. Making a new git branch with the command `git checkout -b`

- -b flags helps create and change into the the new branch

![Alt text](<images/create new branch.png>)

2. Listing the git branches using the command `git branch`

![Alt text](<images/git branch.png>)

3. Change into an Old Branch with command `git checkout <branch-name>`

![Alt text](<images/git checkout.png>)

4. Merging a Branch into another Branch with command `git merge`

![Alt text](<images/git merge.png>)

5. Deleting a git branch with command ` git branch -d <branch_name>`

![Alt text](<images/delete branch.png>)

## **Collaboration and Remote Repositories**

1. Create a github account

![Alt text](<images/git account.png>)

2. Creating a new Repository

![Alt text](<images/new repo.png>)

3. Pushing Local Git Repository to Remote Github Repository

- Add a remote repository to the local repository using command `git remote add origin <link to your github repo>`

![Alt text](<images/git remote add origin.png>)

- After committing changes in local repo. Push the content to the remote repo using the command `git push origin <branch name>`

![Alt text](<images/git push origin main.png>)

4. Cloning Remote Git Repository with command `git clone <link to your remote repository>`

![Alt text](<images/git clone.png>)

## **Branch Management and Tagging**

### Introduction to Markdown Syntax

1. Headings: To create heading

# Heading 1

## Heading 2

### Heading 3

![Alt text](images/heading.png)

2. Emphasis: asterisks or underscore is used to Emphasis text

_italic_ or _italic_

**bold** or **bold**

![Alt text](<images/bold & asterisks.png>)

3. Lists: markdown has support for both ordered and unorded list

   ordered list example:

- Item 1
- Item 2
- Item 3

![Alt text](<images/ordered list.png>)

unordered list example:

1. First item
2. Second item
3. Third item

![Alt text](<images/unordered list.png>)

4. Links: To create hyperlink

   [visit darey.io](https://www.darey.io)

![Alt text](images/links.png)

5. Images: To display an image

![Alt text](<images/git image.jpg>)

![Alt text](images/image.png)

6. Code: To display or code snippets

   `console.log('Welcome to darey.io')`

![Alt text](images/code.png)
