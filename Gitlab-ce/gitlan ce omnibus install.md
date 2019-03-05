# GitLab CE omnibus 설치 

[![Version](https://img.shields.io/badge/version-2019.03.05-red.svg)](./CHANGELOG)  [![License](https://img.shields.io/github/license/mashape/apistatus.svg)](./LICENSE)

* 본 연구실은 Gitlab CE 버전을 사용하고 있습니다. 
* 설치방법은 다양하게 존재하지만 Ubuntu 18.04.2에서 설치하는 방법을 기술하겠습니다. 
  *  설치링크는 https://about.gitlab.com/install/에서 본인의 사용환경에 맞춰서 설치하셔야 합니다. 



### Omnibus package installation (recommended) 

* 필요 의존성 파일 설치

  ```bash
  $ sudo apt-get update && sudo apt upgrade -y 
  $ sudo apt-get install -y curl openssh-server ca-certificates
  ```

* 이메일 전송을 위한 package 설치

  ```bash
  $ sudo apt install postfix -y 
  ```

  * 본 패키지 설치시 기본설정으로 진행합니다. 

* GitLab CE package repository 추가 및 설치 

  ```bash
  $ curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | sudo bash
  ```

* GitLab 재설정 진행하기

  ```bash
  $ sudo gitlab-ctl reconfigure
  ```

  

### GitLab URL 설정하기 

* GitLab 설치를 완료했으면 접속을 위한 도메인을 설정해야 합니다.

* 개인 혹은 소규모 단체에서 가지고 있는 도메인이 있다면 GitLab 설정에서 간단하게 도메인 설정을 진행할 수 있습니다. 

  ```bash
  $ sudo vi /etc/gitlab/gitlab.rb	
  ```

  ```bash
  ## GitLab URL
  ##! URL on which GitLab will be reachable.
  ##! For more details on configuring external_url see:
  ##! https://docs.gitlab.com/omnibus/settings/configuration.html#configuring-the-external-url-for-gitlab
  
  external_url 'http://gitlab.example.com'
  ```

  ```bash
  $ sudo gitlab-ctl reconfigure
  ```



### HTTPS 안전한 연결 구축하기 

* 웹사이트에 접속에 있어서 기존 HTTP 방식은 암호화 되지 않으므로 꾸준히 보안 문제가 지적되어 왔습니다.

* 대부분의 사이트가 HTTPS를 지원하면서, 크롬, 파이어폭스와 같은 웹 브라우저도 HTTPS가 적용되어 있지 않으면 안전하지 않은 연결이라는 경고메시지를 의무적으로 나타나게 됩니다. 

* HTTPS를 적용하기 위해서는 인증서 발급이 필수적이나, 소규모 인원에서 사용하는 경우라면 letsencrypt 인증서를 사용하는 것이 적절하며, GitLab 설정을 통해 간단하게 인증서를 발급하고, 주기적으로 인증서 갱신을 시도할 수 있습니다. 

  ![](https://cdn-images-1.medium.com/max/1600/1*Cd2NBjQD8Luwbu1Z23n5QQ.png)

  ```bash
  $ sudo vi /etc/gitlab/gitlab.rb	
  ```

  ```bash
  ## GitLab URL
  ##! URL on which GitLab will be reachable.
  ##! For more details on configuring external_url see:
  ##! https://docs.gitlab.com/omnibus/settings/configuration.html#configuring-the-external-url-for-gitlab
  external_url 'https://gitlab.example.com'
  ```

  ```bash
  ################################################################################
  # Let's Encrypt integration
  ################################################################################
  letsencrypt['enable'] = true
  letsencrypt[''] = [] # This should be an array of email addresses to add as contacts
  # letsencrypt['group'] = 'root'
  # letsencrypt['key_size'] = 2048
  # letsencrypt['owner'] = 'root'
  # letsencrypt['wwwroot'] = '/var/opt/gitlab/nginx/www'
  # See http://docs.gitlab.com/omnibus/settings/ssl.html#automatic-renewal for more on these sesttings
  letsencrypt['auto_renew'] = true
  # letsencrypt['auto_renew_hour'] = 0
  # letsencrypt['auto_renew_minute'] = nil # Should be a number or cron expression, if specified.
  letsencrypt['auto_renew_day_of_month'] = "*/4"
  ```

  ```bash
  $ sudo gitlab-ctl reconfigure
  ```

  



































