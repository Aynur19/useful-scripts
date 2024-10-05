Инструкция по добавлению второго удаленного репозитория для выполнения push сразу в несколько удаленных репозиториев

Замечание: все команды приведены в качестве реальных примеров, не забывайте указывайть свои ссылки репозиторием и названия веток!

1. Сначала клонируем основной удаленный репозиторий

```shell
git clone git@github.com:Aynur19/leetcode-problems.git	
```

1.1. Если после клонирования у нас нет меток `origin/HEAD` и `origin/main` - то скорее все репозиторий только был создан, и там пока что нет никаких веток и коммитов
Если нужно чтоб были эти указатели, то сначала нужно выполнить `push` коммита в основной репозиторий а заттем выполнить следующую команду чтобы "вытянуть" все изменения по ветке с основного репозитория:

```shell
git fetch origin
```

После данной команды должен появиться указатель на `origin/main`

1.2. Выполняем команду для привязки указателя `HEAD` к основной ветке `main`:

```shell
git remote set-head origin -a
```

2. Добавляем ссылку на второй удаленный репозиторий: 

```shell
git remote add gitlab git@gitlab.com:Aynur19/leetcode-problems.git
```

3. Затем прописываем настройку для выполнения `push` сразу в несколько репозиториев в файле конфигурации гита данного репозитория `./.git/config`:

```properties
[remote "all"]
	url = git@github.com:Aynur19/leetcode-problems.git	
	url = git@gitlab.com:Aynur19/leetcode-problems.git 
```

4. Добавляем alias-команду для выполнения push во все удаленные репозитории и обновления ссылки на HEAD (`./.git/config`):

```properties
[alias]
    pushall = !git push all && git fetch origin
```


Окончательная версия файла конфигурации гита для данного репозитория ( './.git/config'):
```properties
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true

# Основной удаленный репозиториый (создается автоматически при клонировании)
[remote "origin"]
	url = git@github.com:Aynur19/leetcode-problems.git
	fetch = +refs/heads/*:refs/remotes/origin/*

# основная ветка (создается автоматически при клонировании)
[branch "main"]
	remote = origin
	merge = refs/heads/main

# дополнительный удаленный репозиторий, создается путем выполнения команды:
# git remote add gitlab git@gitlab.com:Aynur19/leetcode-problems.git
[remote "gitlab"]
	url = git@gitlab.com:Aynur19/leetcode-problems.git
	fetch = +refs/heads/*:refs/remotes/gitlab/*

# дополнительная команда, прописывается вручную и применяется так:
# git push all
[remote "all"]
	url = git@github.com:Aynur19/leetcode-problems.git	
	url = git@gitlab.com:Aynur19/leetcode-problems.git

# дополнительная alias-команда - выполняет пуш во все удаленные репозитории и обновляет ссылку на HEAD и origin.
# прописывается в этом файле вручную. Перед этим лучше выполнить команду 
# git remote set-head origin -a
# иначе ссылка на HEAD не будет обновляться
[alias]
    pushall = !git push all && git fetch origin
```
