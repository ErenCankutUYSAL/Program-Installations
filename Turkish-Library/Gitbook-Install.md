## GitBook

Opened for Gitbook Projects and automated sync tests.

#### Gitbook Installation

Install this globally and you'll have access to the gitbook command anywhere on your system.

```
$ npm install -g gitbook-cli
```
Note: The purpose of the gitbook command is to load and run the version of GitBook you have specified in your book (or the latest one), irrespective of its version. The GitBook CLI only support versions >=2.0.0 of GitBook.

gitbook-cli store GitBook's versions into ~/.gitbook, you can set the GITBOOK_DIR environment variable to use another directory.

###### How to install it?

```
$ npm install -g gitbook-cli
```
###### How to use it?
Run GitBook
Run command gitbook build, gitbook serve (read GitBook documentation for details).

List all available commands using:
```
$ gitbook help
```
Specify a specific version
By default, GitBook CLI will read the gitbook version to use from the book configuration, but you can force a specific version using --gitbook option:
```
$ gitbook build ./mybook --gitbook=2.0.1
```
and list available commands in this version using:
```
$ gitbook help --gitbook=2.0.1
```
Manage versions
List installed versions:
```
$ gitbook ls
```
List available versions on NPM:
```
$ gitbook ls-remote
```
Install a specific version:
```
$ gitbook fetch 2.1.0
```
or a pre-release
```
$ gitbook fetch beta
```
Update to the latest version
```
$ gitbook update
```
#### Using Gitbook Book.json

When gitbook compiles a book, it will read it in the top-level directory of the book's source code.

#### Content Management

It shows how to synchronize and manage the files in the /opt folder on gitbook.medyatakip.com:4000.

`scp -r` is used as copy command on gitlab-ci.yml in this project.

The copy command works like this:
```
scp -r ./archive/ root@gitbook.medyatakip.com:/opt/gitbook/archive/"$CI_PROJECT_NAME".md
```

In this command, the first line after `scp -r` specifies the file in its own folder and if we want to get all the contents of this file, we just need to write the file name.

The second line, before the `@` specifies which user to connect to the remote linux. After the `@` specifies the path to the remote linux folder.

We set the name of the `readme.md` file created in the `"$CI_PROJECT_NAME".md` section as the project name and send it accordingly.

##### After copy operations and sync operations

Gitbook linux is connected and the commands are run in order by going under the `/opt/gitbook` folder where the files are created:
```
gitbook build
cp -R /opt/gitbook/SUMMARY.md /opt/gitbook/README.md

```

We ensure that the files transferred with the`gitbook build` process are synchronized on `summary.md` and adjusted automatically.

With the `cp -R` command, we ensure that the changes in the summary.md are synchronized into the introduction.

Thanks to the `gitbook serve` command, we run `gitbook` again.
