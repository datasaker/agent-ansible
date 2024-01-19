# Ansible Datasaker Role

Ansible을 이용하여 Datasaker Agent를 설치할 수 있습니다.

## Requirements

- Ansible v2.6+가 필요합니다.
- 대부분의 Debian Linux 배포판을 지원합니다.
- 대부분의 Redhat Linux 배포판을 지원합니다.
- Amazon Linux 2 배포판을 지원합니다.


## Installation

Ansible Galaxy에서 Datasaker role을 설치합니다.

```shell
ansible-galaxy install dsk_bot.datasaker
```

에이전트를 배포하기 위하여 Ansible playbook을 작성합니다.


****`dsk-log-agent` 설치 시 `fluent-bit` 이 자동으로 설치됩니다.*** 


아래는 기본 설치에 대한 예시입니다.

#### Host Agent Default Install Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent","dsk-log-agent"] 
```
#### Docker Agent Default Install Example
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_docker_agents: ["dsk-docker-node-agent","dsk-docker-log-agent"]
```

### 필수 설정

| 변수명                                      | 설명                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`datasaker_api_key`|API Key를 입력합니다.|
|`datasaker_agents`| 각 호스트에 설치하고자 하는 Host Agent 리스트입니다. <br>`dsk-node-agent` `dsk-trace-agent` `dsk-log-agent` `dsk-postgres-agent` `dsk-plan-postgres-agent`<br>| `dsk-node-agent`|
|`datasaker_docker_agents`| 각 호스트에 설치하고자 하는 Docker Container Agent 리스트입니다. <br>Docker Container Agents를 넣으면 Host Agent 설치는 자동으로 비활성화 됩니다. <br>`dsk-docker-node-agent` `dsk-docker-trace-agent` `dsk-docker-log-agent` `dsk-docker-postgres-agent`<br>| `dsk-docker-node-agent`|
<!--
#### Datasaker 공통 설정
| 변수명                                      | 설명                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`datagate_url`| Datasaker Agent가 전송하는 Datasaker Datagate URL 설정. <br>| `gate.kr.datasaker.io`|
|`datagate_trace_url`| `dsk-trace-agent` Datagate URL 설정. <br>| `datagate_url`|
|`datagate_trace_port`| `dsk-trace-agent` Datagate Port 설정. <br>| `31300`|
|`datagate_trace_timeout`| `dsk-trace-agent` Data 전송 만료 시간 설정. <br>| `5s`|
|`datagate_manifest_url`| `dsk-manifest-agent` Datagate URL 설정. <br>| `datagate_url`|
|`datagate_manifest_port`| `dsk-manifest-agent` Datagate Port 설정. <br>| `31301`|
|`datagate_manifest_timeout`| `dsk-manifest-agent` Data 전송 만료 시간 설정. <br>| `5s`|
|`datagate_metric_url`| `dsk-metric-agent` Datagate URL 설정. <br>| `datagate_url`|
|`datagate_metric_port`| `dsk-metric-agent` Datagate Port 설정. <br>| `31302`|
|`datagate_metric_timeout`| `dsk-metric-agent` Data 전송 만료 시간 설정. <br>| `5s`|
|`datagate_loggate_url`| `dsk-log-agent` Datagate URL 설정. <br>| `datagate_url`|
|`datagate_loggate_port`| `dsk-log-agent` Datagate Port 설정. <br>| `31304`|
|`datagate_loggate_timeout`| `dsk-log-agent` Data 전송 만료 시간 설정. <br>| `5s`|
|`datasaker_api_url`| Datasaker API Server URL. <br>| `api.kr.datasaker.io`|
|`datasaker_api_send_interval`| Datasaker API Server Data 전송 만료 시간 설정. <br>| `1m`|
-->

### Docker Container Agent 설정
| 변수명                                      | 설명                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`datasaker_docker_config_path`| Datasaker Global Config 위치 설정. <br> | `~/.datasaker`|
|`datasaker_docker_global_config`| Datasaker Global Config 이름 설정. <br> | `~/.datasaker/config.yml`|
|`docker_default_path`| Datasaker Docker Log Agent에 마운트할 Docker Log 수집 위치 설정. <br> | `/var/lib/docker/containers/`|
|`datasaker_docker_path`| Datasaker Docker Agent Container 위치 설정. <br> | `/var/datasaker`|
|`container_agent_restart_policy`| `dsk-container-agent` Container Restart Policy 설정. <br> | `always`|
|`node_agent_restart_policy`|  `dsk-node-agent` Container Restart Policy 설정. <br> | `always`|
|`trace_agent_restart_policy`|  `dsk-trace-agent` Container Restart Policy 설정. <br> | `always`|
|`log_agent_restart_policy`|  `dsk-log-agent` Container Restart Policy 설정. <br> | `always`|
|`postgres_agent_restart_policy`|  `dsk-postgres-agent` Container Restart Policy 설정. <br> | `always`|
|`container_agent_log_level`| `dsk-container-agent` Log Level 설정. <br> | `INFO`|
|`node_agent_log_level`| `dsk-node-agent` Log Level 설정. <br> | `INFO`|
|`trace_agent_log_level`| `dsk-trace-agent` Log Level 설정. <br> | `INFO`|
|`log_agent_log_level`| `dsk-log-agent` Log Level 설정. <br> | `INFO`|
|`postgres_agent_log_level`| `dsk-postgres-agent` Log Level 설정. <br> | `INFO`|

<!--|`datasaker_docker_user`| Datasaker Docker Container Directory Ownership 설정. <br> | `datasaker`|
|`datasaker_docker_group`| Datasaker Docker Container Directory Group 설정. <br> | `datasaker`|
|`datasaker_docker_user_uid`| Datasaker Docker Container Agent User UID 설정 <br> | `202306`|
|`datasaker_docker_user_gid`| Datasaker Docker Container Agent User GID 설정 <br> | `202306`|
|`container_agent_image_tag`| `dsk-container-agent` Image tag 설정. <br> | `latest`|
|`node_agent_image_tag`| `dsk-node-agent` Image tag 설정. <br> | `latest`|
|`trace_agent_image_tag`| `dsk-trace-agent` Image tag 설정. <br> | `latest`|
|`log_agent_image_tag`| `dsk-log-agent` Image tag 설정. <br> | `latest`|
|`postgres_agent_image_tag`| `dsk-postgres-agent` Image tag 설정. <br> | `latest`| -->

### Datasaker Agent 상세 설정
- Host Agent 와 Docker Container Agent는 같은 설정값을 사용합니다.
- mysql-agent 와 plan-mysql-agent는 같은 설정값을 사용합니다.
- maria-agent 와 plan-maria-agent는 같은 설정값을 사용합니다.

| 변수명                                      | 설명                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`trace_sampling_rate`| `dsk-trace-agent` 에서 collector에 적용되는 샘플링 비율 설정.<br>- 100 이상일 때 모든 데이터가 수집.<br> | `10`|
|`logs[*].service`|로그 수집 대상의 서비스 이름 설정.|`default`|
|`logs[*].tag`|로그 수집 대상의 태그 설정.|`None`|
|`logs[*].keyword`|로그 수집 키워드 설정. 키워드가 포함된 로그만 수집.|`None`|
|`logs[*].multiline.format`|멀티라인 로그 포맷 설정 (예 : go, java, ruby, python).|`None`|
|`logs[*].multiline.pattern`|멀티라인 로그 패턴 설정. (예 : ^\d{4}-\d{2}-\d{2}).|`None`|
|`logs[*].masking[*].pattern`|마스킹할 로그 패턴 설정. (예 : ^\d{4}-\d{2}-\d{2}) 사용자 커스텀 정규식 패턴 사용 가능.|`None`|
|`logs[*].masking[*].replace`|마스킹 패턴이 대체될 문자열 설정. (예 : ******).|`None`|
|`logs[*].collect.type`|로그 수집 방법 설정 (`file`, `driver` 중 하나의 값을 작성).|`file`|
|`logs[*].collect.category`|서비스 분류 설정 (`app`, `database`, `syslog`, `etc` 중 하나의 값을 작성).|`etc`|
|`logs[*].collect.address`|데이터베이스 host 및 port 정보 설정 (서비스 분류가 database인 경우 설정).|`None`|
|`logs[*].collect.file.paths`|로그 수집 대상 경로 설정. 예 : /var/log/sample/*.log.|`['/var/log/*.log']`|
|`logs[*].collect.file.exclude_paths`|로그 수집 제외 대상 경로 설정.|`None`|
|`custom_log_volume`| Docker 사용 시 수집할 로그가 있는 경로 마운트.|`/var/lib/docker/containers/`|
|`postgres_user_name`| `dsk-postgres-agent`에 Postgres user ID 설정. <br> | `None` |
|`postgres_user_password`| `dsk-postgres-agent`에 Postgres user password 설정. <br> | `None` |
|`postgres_database_address`| `dsk-postgres-agent`에 Postgres address 설정. <br> | `None` |
|`postgres_database_port`| `dsk-postgres-agent`에 Postgres port 설정. <br> | `None` |
|`reids_address`|`reids_address`에 redis address 설정.|`-`|
|`redis_agent_port`|`redis_agent_port`에 redis agent port 설정.|`19121`|
|`redis_user`|`redis_user`에 redis user 설정. (없을 경우 생략)|`-`|
|`redis_pass`|`redis_pass`에 redis user password 설정. (없을 경우 생략)|`-`|
|`aws_access_key_id`|`aws_access_key_id`에 cloudwatch agent 사용을 위한 key id 설정.|`-`|
|`aws_secret_access_key`|`aws_secret_access_key`에 cloudwatch agent 사용을 위한 access key 설정.|`-`|
|database|모든 database agent에 사용되는 공통 변수 (아래 참고)|`-`|

```yaml
    maria: # maria or mysql or postgres
      agent_name: ''
      agent_cluster: ''
      db:
        - host: '' # database 호스트의 주소
          port: '' # database port
          user_name: '' # database 유저
          user_password: '' # database 유저 패스워드
          database: '' # 대상 database
          ssl_skip: '' # ssl skip 여부 (default : false)
          ssl_ca: '' # ca 파일 경로
          ssl_cert: '' # cert 파일 경로
          ssl_key: '' # key 파일 경로
          ssl_tls: '' # tls 여부 (default : false)
          interval_enabled: '' # interval 활성화 여부
          scrape_interval: '' # scrape interval 설정 (default : 5s)
          append_db:
            - database: '' # 대상 database
              long_session: '' # long session 설정 (default : 5s)
              
# ssl_skip 값이 false 일 경우 ssl_* 변수들 생략 가능
# interval_enabled 값이 false 일 경우 scrape_interval , append_db 생략 가능
```

#### Ansible Playbook 상세 설정 Example (Linux)
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent","dsk-maria-agent","dsk-log-agent"]
    maria:
      agent_name: ''
      agent_cluster: ''
      db:
        - host: ''
          port: ''
          user_name: ''
          user_password: ''
          database: ''
          ssl_skip: ''
          interval_enabled: ''
          scrape_interval: ''
          append_db:
            - database: ''
              long_session: ''
    logs:
    - collect:
        type: file
        file:
          paths:
          - /var/log/*.log
```

#### Ansible Playbook 상세 설정 Example (Docker)
```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_api_key: "<YOUR_API_KEY>"
    datasaker_agents: ["dsk-node-agent","dsk-maria-agent","dsk-log-agent"]
    maria:
      agent_name: ''
      agent_cluster: ''
      db:
        - host: ''
          port: ''
          user_name: ''
          user_password: ''
          database: ''
          ssl_skip: ''
          interval_enabled: ''
          scrape_interval: ''
          append_db:
            - database: ''
              long_session: ''
    logs:
    - collect:
        type: file
        file:
          paths:
          - /var/log/*.log
          - /var/lib/docker/containers/*/*.log
  custom_log_volume:
    - /var/log/
    - /var/lib/docker/containers
```


## Uninstallation

Datasaker Agent를 제거 할 수 있습니다. 
datasaker_clean은 uninstall이 `True`로 설정되어야 합니다.

| 변수명                                      | 설명                                      | Default                                      |
|--------------------------------------------|--------------------------------------------------|--------------------------------------------------|
|`uninstall`| `datasaker_agents` 또는 `datasaker_docker_agents` 에 작성된 Agent만 제거. <br> | `False`|
|`datasaker_clean`|	`datasaker_agents` 또는 `datasaker_docker_agents` 에 작성된 Agent 와 생성 된 폴더 및 설정 파일까지 제거.  <br> | `False`|

#### Datasaker Agents Uninstall Example

```yml
- hosts: servers
  become: true
  roles:
    - role: dsk_bot.datasaker
  vars:
    datasaker_agents: ["<AGENT_NAME>"]
    uninstall: True
    datasaker_clean: True
```
<!--
## Troubleshooting

### Debian stretch

**Note:** 
-->
