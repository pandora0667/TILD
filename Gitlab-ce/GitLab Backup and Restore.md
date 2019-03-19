# GItlab 백업 및 복원

![](https://docs.gitlab.com/ee/raketasks/backup_hrz.png)

*  한밭대학교 무선통신소프트위어연구실(Wisoft) 에서는 형상관리를 git으로 관리하고 있습니다. 
*  본 연구실에서 운영중인 서비스들은 가상화 환경으로 전환한 이후 생성된 데이터를 백업하고 관리하는 과정이 매우 중요한 과제가 되었습니다. 
   *  이러한 문제점을 해결하기 위해서 주기적이고 안정적인 백업 환경 구축, 자동화된 환경설계가 필요하게 되었습니다. 
   *  현재 운영중인 대부분의 서버들은 Ansible를 사용하여 주기적인 업데이트와 백업을 진행하고 있습니다.  

---



### GitLab Backup 

* GitLab에 대한 모든 설정은 **/etc/gitlab/gitlab.rb**에서 관리할 수 있습니다.  

  * **gitlab.rb** 파일에서 자세한 설정을 원하는 경우 다음 링크를 통해서 확인하시길 바랍니다. 

    * https://docs.gitlab.com/ee/raketasks/backup_restore.html

    

* 보편적인 방법으로 Omnibus package로 설치한 경우 백업은 다음과 같습니다. 

  ```bash
  $ sudo gitlab-rake gitlab:backup:create
  ```

  ```
  Dumping database tables:
  - Dumping table events... [DONE]
  - Dumping table issues... [DONE]
  - Dumping table keys... [DONE]
  - Dumping table merge_requests... [DONE]
  - Dumping table milestones... [DONE]
  - Dumping table namespaces... [DONE]
  - Dumping table notes... [DONE]
  - Dumping table projects... [DONE]
  - Dumping table protected_branches... [DONE]
  - Dumping table schema_migrations... [DONE]
  - Dumping table services... [DONE]
  - Dumping table snippets... [DONE]
  - Dumping table taggings... [DONE]
  - Dumping table tags... [DONE]
  - Dumping table users... [DONE]
  - Dumping table users_projects... [DONE]
  - Dumping table web_hooks... [DONE]
  - Dumping table wikis... [DONE]
  Dumping repositories:
  - Dumping repository abcd... [DONE]
  Creating backup archive: $TIMESTAMP_gitlab_backup.tar [DONE]
  Deleting tmp directories...[DONE]
  Deleting old backups... [SKIPPING]
  ```



### GitLab Restore

* 생성된 백업파일은 기본적으로 **/var/opt/gitlab/backups**에 저장됩니다. 
  * 로컬에 저장된 백업파일은 100% 안전하다고 할 수 없기때문에 반드시 외부 환경에 소산 보관해야 합니다. 
  * GitLab서버와 FreeNAS와 같은 외장 스토리지 서버와 네트워크로 연결하는 (CIFS 등) 방법도 고려해야 합니다.  
    * 네트워크 환경에서 백업 환경을 구축할 때, 대규모 환경에서는 SAN으로 구성하는 것이 좋은 대안이 될 수 있으나 소규모 환경에서는 비용문제가 많이 발생할 수 있기 때문에 내부 네트워크 환경에서 시행하는 것이 좋습니다. 
    * 단, 같은 네트워크 환경에서 백업이 같이 이루어질 경우 네트워크 장비의 용량에 따라 내부 네트워크의 전체 속도가 떨어질 수 있기 때문에 네트워크 사용량이 적은 시간에 진행하거나 네트워크를 분리하는 방법도 고려하는 것이 좋습니다. 



#### GitLab Service 중단 및 백업

```bash
$ sudo gitlab-ctl stop unicorn
$ sudo gitlab-ctl stop sidekiq
# Verify
$ sudo gitlab-ctl status
```

```bash
# 백업 파일이 하나일 경우
$ sudo gitlab-rake gitlab:backup:restore 

# 백업 파일이 여러개 있을 경우 
$ sudo gitlab-rake gitlab:backup:restore BACKUP=1493107454_2018_04_25_10.6.4-ce
```

* 기본적인 백업 디렉토리에 백업 파일이 존재하면 자동으로 기존에 있던 데이터베이스가 삭제되고, 백업데이터로 변경됩니다. 
  * 새롭게 설치한 GitLab 서버에도 다음과 같이 진행하면 기존에 사용하던 환경으로 마이그레이션이 완료됩니다. 
* 이때, 백업 당시에 사용했던 GitLab 버전과 사용중인 GitLab 버전이 다를 경우에는 복구가 진행되지 않기 때문에 버전과 동일한 백업 파일을 생성하여 진행할 수 있도록 합니다. 



#### GitLab Service 시작 

```bash
$ sudo gitlab-ctl restart
$ sudo gitlab-rake gitlab:check SANITIZE=true
```

* 중단된  서비스를 재시작하여 정상적으로 동작하는지 확인합니다. 

