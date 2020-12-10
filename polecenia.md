
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