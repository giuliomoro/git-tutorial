# Resources for a basic barebone git tutorial

## HOW DID WE GET HERE? git log
```
git log --graph --oneline --decorate --all
```

```
git log --graph --oneline --decorate --all --color=always | sed "s/\(.\{,200\}\).*/\1/" | less -r
```

Use FileMerge as mergetool https://gist.github.com/bkeating/329690

## BELA WORKFLOW
## (or more in general: what to do when your repo does not have internet acces)

### Setup:

#### Bela:

Use the GUI to create and commit

#### Hosting service

On your favourite git hosting service (e.g.: github.com), create a new repo and grab its URL

#### On the host computer

```
mkdir myproject
cd myproject
git init
git remote add board root@bela.local:Bela/projects/myproject
git remote add origin git@github.com:giuliomoro/myproject
git fetch board
git checkout master
git push board
```

### Every time you want to synchronize

#### push from the board to a remote

Run this from the host:
```
git pull board master
git push origin master
```

### pull to board from origin

Run this on the host:
```
git pull origin master
git push board master:tmp
```
then get onto the board:
```
ssh root@bela.local
cd Bela/projects/myproject
git merge tmp
git branch -D tmp
```

## WHO, WHY AND WHEN DID (I) DO THIS, AGAIN? git blame

## WHEN DID I BREAK THIS? git bisect

## I HATE TYPING MY USERNAME AND PASSWORD FOR EVERY PUSH ssh keys

