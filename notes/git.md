[< back](../README.md)
# GIT 
![](../img/git.jpg)

### Start
- **klonovat** adresar

    `git clone <repo> <adresar>`


- **inicializacia** - nastavenie pracovneho adresara ako git repo

    `git init`

- pridanie lokalneho projektu do prazdneho repa

    ```git add .
    git commit -m "first commit"
    git remote add origin <remote repository URL>
    git push -u origin master
    ```
### Get info

- **vetva a zmeny**

    `git status`

- **len zmeny**

    `git status -s`

- **vypis unstagged zmien** (rozdiel oproti poslednemu commitu)

    `git diff`

- *vypis stagged zmien** (rozdiel oproti poslednemu commitu)

    `git diff --staged`

- **historia**

    `git log`

- **graf**

    `git log --oneline --graph --decorate --all -n 20`
-all znamena ze chcem vsetky vetvy

### Ukladanie zmien
- **pridanie zmien do indexu** - stagged changes

    `git add <path>`

- **odobranie z indexu**

    `git reset <path>`

- **commit**

    `git commit -m "message"`

- fast commit

    `git commit -m-a "sprava"` -pred commitom urobi `git add .`

- multi-line commit

    `git commit -m $'daco\nvolaco'`

- pridanie zmien do posledneho commitu

    `git commit --amend --no-edit` -potom force-push

- **push**

    `git push` -na aktualnu vetvu

    `git push origin <vetva>` -na inu vetvu

### Nacitanie zmien z remote repo

- **fetch**

    `git fetch` -aktualizuje graf (info o zmenach) ale nesynchronizuje lokalny adresar

- **pull**

    `git pull` -aktualizuje a synchronizuje lokalny adresar (fetch + rebase)

### Vetvy

- **prepnut vetvu**

    `git checkout <vetva>`

- **vytvorit vetvu**

    `git branch <vetva>`

- **vytvorit vetvu a zostat na nej**

    `git checkout -b <vetva>`

- **vypisat aktualnu vetvu**

    `git branch`

- **vypisat vetvy**

    `git branch -a`

- **vymazat lokalnu vetvu**

    `git branch -d <vetva>`

- nutene vymazanie

    `git branch -D <vetva>`

- **vymazat remote vetvu**

    `git push remote --delete <vetva>`

    alebo

    `git push origin :<vetva>`

### Merge
<img src="../img/merge.png" width="400">

### Rebase
<img src="../img/rebase.png" width="400">

### Stash

### Alias

vytvori alias prikazu

`git config --global alias.moj_alias 'zastupeny prikaz'`