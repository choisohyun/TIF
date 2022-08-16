## Git remote 레포지토리 추가하여 다른 플랫폼으로 Fork 뜨기

현재 remote는 origin만 있습니다.

```
$ git remote -v
origin  https://~.git (fetch)
origin  https://~.git (push)
```

Fork 뜨고자 하는 레포지토리를 임의의 이름으로 추가합니다.

- 여기서는 github로 이름을 지었습니다.

```
$ git remote add github https://@.git
```

remote 다시 확인합니다.

```
$ git remote -v
github  https://@.git (fetch)
github  https://@.git (push)
origin  https://~.git (fetch)
origin  https://~.git (push)
```


Git remote에 있는 모든 브랜치 로컬로 받아옵니다.

- 참고글: https://stackoverflow.com/questions/67699/how-do-i-clone-all-remote-branches
```
git config --global alias.clone-branches '! git branch -a | sed -n "/\/HEAD /d; /\/master$/d; /remotes/p;" | xargs -L1 git checkout -t'\n
git clone-branches
```

Git local로 가지고 있는 브랜치를 모두 올립니다.
```
git push --all {remote name}
```


