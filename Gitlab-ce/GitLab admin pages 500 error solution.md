# GitLab Runner 500 Error 해결방법 

* 본 연구실에서는 GitLab CE 버전이 설치되어 있는데 새로운 서버의 도입으로 인해서 마이그레이션을 진행 후에 Admin 페이지에서 GitLab Runner 페이지가 500 Error가 뜨면서 사용할 수 없는 상태에 이르렀다. 

![](https://forum.gitlab.com/uploads/default/original/2X/5/53d0d3a0e91bca5d884629188f5aeebe13f32a42.png)

* 이러한 문제는 기존 키값의 충돌로 인한 문제로서 GItLab 옴니버스로 설치했을 시에 발생했다는 사실을 밝힌다. 관련문제가 흔하게 발생하는 문제가 아닌 만큼 아래와 같이 기존 GitLab Runner 키값을 삭제해 주고 키값을 재생성 한 다음에 Runner를 다시 등록해 주는 과정을 거치면 문제를 해결할 수 있다.  

```bash
root@gitlab:/# gitlab-rails console
-------------------------------------------------------------------------------------
 GitLab:       11.5.1 (c90ae59)
 GitLab Shell: 8.4.1
 postgresql:   9.6.8
-------------------------------------------------------------------------------------
Both Deployment and its :status machine have defined a different default for "status". Use only one or the other for defining defaults to avoid unexpected behaviors.
Loading production environment (Rails 4.2.10)
irb(main):001:0> ApplicationSetting.current.reset_runners_registration_token!
=> true
irb(main):002:0> 
```

* 위와 같이 명령어를 그대로 입력하면 기존 인증값이 삭제되고 다시 접속하면 정상적으로 사용할 수 있다. 만약 기존 GitLab 서버를 마이그레이션 완료하고 이와같은 에러가 발생한다면 위와같이 시도할 것을 추천한다. 

- 문제가 흔하게 발생하지 않는 만큼 정보가 거의 없어서 해결책에 대한 키워드를 검색하는데 오랜 시간이 걸렸다. stak overflow에서 관련 해결 방법이 공유된 만큼 아래 링크에서 확인할 것을 추천한다. 본 연구실에서는 이 방법을 통해 본래 상태로 원복할 수 있었다. 
  - https://stackoverflow.com/questions/54128023/gitlab-500-errors-in-the-admin-area