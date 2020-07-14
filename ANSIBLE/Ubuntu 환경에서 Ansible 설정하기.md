# Ubuntu 환경에서 Ansible 설정하기 



### Ansible 설치하기 

```bash
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
$ sudo apt update
$ sudo apt install ansible -y 
```



### Ansible 설정 파일 

```bash
$ sudo vi /etc/ansible/ansible.cfg
```

```bash
[defaults]
host_key_checking = False
```

* Ansible 1.2.1 이후 버전부터는 호스트 키 확인 과정이 디폴트로 활성화 됩니다.
*  만약 호스트가 새로 설치 되었고 known_hosts 파일에 다른 키가 등록된다면 키 충돌 에러 메시지가 나타납니다. 
* 만약 호스트가known_hosts파일에 등록되지 않은 시점이라면 이 키를 등록할것인지에 대한 확인 프롬프트가 나타납니다. 
* 이를 방지하기 위해서 위와같이 설정을 변경합니다. 

```bash
$ sudo vi /etc/ansible/hosts
```

```bash
[docker]
192.168.10.22
192.168.10.23
192.168.10.24
```



### Ansible ping 테스트 진행하기 

```bash
$ ansible all -m ping -k -u wisoft
```

```bash
192.168.10.23 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
192.168.10.24 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
192.168.10.22 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```



### Ansible로 서버에 echo 명령 실행하기 

```bash
$ ansible all -a "/bin/echo hello" -k -u wisoft
```

```bash
192.168.10.23 | CHANGED | rc=0 >>
hello

192.168.10.22 | CHANGED | rc=0 >>
hello

192.168.10.24 | CHANGED | rc=0 >>
hello
```

