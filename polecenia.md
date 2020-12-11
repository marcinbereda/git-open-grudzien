
# VsCode
Plik => Otwórz Folder => Tworzymy Folder => Wchodzimy do środka => klikamy "Select folder"
File => Open Folder => Tworzymy Folder => Wchodzimy do środka => klikamy "Select folder"

Terminal => New Terminal
Powershell => Select detault shell => Git Bash
Kill terminal (śmietnik)  i Terminal => New Terminal

# Instalacje
https://git-scm.com/download/win

git --version
git version 2.29.2.windows.2

# Nowe repozytorium
git init 
git status

# Konfiguracja globalna
<!-- C:\Users\<nazwa usera>\.gitconfig -->

git config --global user.name "Mateusz Kulesza"
git config --global user.email "mateusz@altkom"
git config --list --global

# Lokalna konfiguracaj (tylko to repo)
<!-- ./.git/config -->
git config user.name "Mateusz Kulesza"
git config user.email "mateusz@altkom"
git config --list

# Pierwszy commit
git status 
git add test.txt
git status
git commit -m "Pierwszy commit"
git status

# Vim 
- a - append - wprowadzanie
- EXC - wyjdz z wprowadzania tekstu
- :wq - write + quit
- :q! - wyjdz bez zapisywania 
https://149351115.v2.pressablecdn.com/wp-content/uploads/2017/05/exitvim-1200x534.png

# Przegladanie
git status
git log 
git show 91a42b94

git log --reverse
git log -2

# Internals vs PLumbing - nie rób tego w domu (ani pracy!)
<!-- git add -->
git hash-object test.txt
30d74d258442c7c65512eafab474568dd706c430

git cat-file -p 30d74d258442c7c65512eafab474568dd706c430 > test.txt

git update-index --add --cacheinfo 100644 30d74d258442c7c65512eafab474568dd706c430 test.txt

git status

<!-- git commit -->
git write-tree
095a057d4a651ec412d06b59e32e9b02871592d5

<!-- 100644 -- example linux file mode -->

git update-index --add --cacheinfo 100644 30d74d258442c7c65512eafab474568dd706c430 test2.txt

git write-tree
df09887a12bbe162a9b4cf839c191d350b609010

git read-tree 095a057d4a651ec412d06b59e32e9b02871592d5
git read-tree --prefix=podkatalog df09887a12bbe162a9b4cf839c191d350b609010

git write-tree
9887edef9c41a979cca8df000faa897f871ac614

git cat-file -p 9887edef9c41a979cca8df000faa897f871ac614
040000 tree df09887a12bbe162a9b4cf839c191d350b609010    podkatalog
100644 blob 30d74d258442c7c65512eafab474568dd706c430    test.txt

git commit-tree 9887edef9c41a979cca8df000faa897f871ac614 -m "Pierwszy commit - internals" -p master

git cat-file -p 2bedcf015245136d96dc46bae7ca15e1aadd6995
<!-- tree 9887edef9c41a979cca8df000faa897f871ac614
parent f855671c55a8c1e35f974df82128a9e80b896014
author Mateusz Kulesza <mateusz@altkom> 1607599400 +0100
committer Mateusz Kulesza <mateusz@altkom> 1607599400 +0100

Pierwszy commit - internals -->

# Commit amend
git add zmieniony.plik
git commit --amend 
<!-- VIM - poprawiamy message + :wq -->

git commit --amend  -m "poprawiona nazwa"

# Committer vs Author
git show --format=fuller 88232a771a0e91
commit 88232a771a0e915200a86dfe8b2437b702c5f3af (HEAD -> master)
Author:     Mateusz Kulesza <mateusz@altkom>
AuthorDate: Thu Dec 10 13:40:48 2020 +0100
Commit:     Mateusz Kulesza <mateusz@altkom>
CommitDate: Thu Dec 10 13:45:20 2020 +0100

# Alias
git config alias.log1 "log --oneline"
git config --global alias.log1 "log --oneline"

https://github.com/GitAlias/gitalias

# Removing files
remove file manually 
git add removed.file.txt
git status // file removed
git commit 

git rm file.to.remove.txt
git status // file removed
git commit 

# Renaming files
git mv strona/git-init.html strona/lokalna/
git mv strona/git-*.html strona/lokalna/

git mv nazwa\ ze\ spacja.txt
git mv "nazwa ze spacja.txt"

# Restoring files
git restore strona/index.html
git checkout ccff80e strona/index.html
git checkout HEAD~3 strona/index.html

git log --name-only
git log -- test.txt --name-only
git show 91a42 --name-only
git checkout 91a42 test.txt
git checkout 91a42 test.txt test2.txt test3.txt
git checkout 91a42 "nazwa ze spacja.txt"

# Switch to commit / branch
git checkout 91a42
git switch 91a42
git checkout 91a42
git switch master

# Reverting commits
git revert ee35126
<!-- Fix conflicts - remove conflict markers <<<<< ======= >>>>> -->
git add confilcted.file.txt
git revert --continue

# Git Stash
git stash  -- stash working dir and index and ONLY tracked files

<!-- Include Untracked files -->
git stash -u 

git stash list
git stash apply stash@{12} - aplikuj
git stash drop stash@{12} - usun

git stash pop - aplikuj i usun ostatni

git show stash@{0}
git diff stash@{0}

# Git Tag
git tag sekcje_skonczone  5b1f7fb
git tag v1.0 -a -m "Wersja 1.0 opublikowana"

git tag inny_tag  HEAD~3

<!--  -->
git log1 -4

8493a84 (HEAD -> master, tag: v1.0) Poprawki do Stash i Diff
5b1f7fb (tag: sekcje_skonczone) Strona - sekcja praca równoległa na gałeziach - stash
<!--  -->
git tag -n

sekcje_skonczone Strona - sekcja praca równoległa na gałeziach - stash
v1.0            Wersja 1.0 opublikowana
<!--  -->
git show v1.0

tag v1.0
Tagger: Mateusz Kulesza <mateusz@altkom>
Date:   Fri Dec 11 10:13:50 2020 +0100

Wersja 1.0 opublikowana

commit 8493a847f8aaf36a14c773e87862606eb3b2ec22 (HEAD -> master, tag: v1.0)
...

# Git branch
<!-- Create -->
git branch wip_header_logo master
git branch wip_header_logo HEAD
git branch wip_header_logo

<!-- Create and switch -->
git checkout -b wip_header_logo
git switch -c wip_header_logo

<!-- Switch to exising branch -->
git checkout wip_header_logo
git switch  wip_header_logo

<!-- Show ALL branches -->
git log --oneline --all --decorate
git log --oneline --all --decorate --graph
<!-- :q - to exit git log -->
<!-- git config alias.graph "log --oneline --all --decorate --graph" -->

git branch -v
  master          fe93654 Polecenia - git tag
* wip-git_branch  fe93654 Polecenia - git tag
  wip_header_logo d4b6bd4 WIP - Header + logo


# Praca zdalna
git clone zdalne_repo lokalny_fork
git remote

git remote show zdalne_repo_naza
git remote show origin

 git remote add local_fork C:/Projects/altkom/git-open-grudzien-fork

 # Pobieranie 
 git fetch origin master <!-- -> origin/master -->
 git merge FETCH_HEAD

 git pull <!-- fetch + merge FETCH-HEAD -->

 # Tracking
 git branch --set-upstream-to=local_fork/<branch> master
 git branch -t local_fork/master
<!-- Branch 'local_fork/master' set up to track local branch 'master'.  -->

<!-- Z inneg zdalnego na inny lokalny -->
 git pull origin zdalny:lokalny
<!-- Z ttego samego zdalnego na ten sam lokalny -->
 git pull origin zdalny_lokalny

<!-- Zapamietaj i śledź zdalny branch -->
 git pull --set-upstream local_fork master
 git pull
