# python-app

// chmod 600 id_rsa (in user Jenkins)

// https://github.com/rufatzakirov/nodejs-sharks

- name: install docker and start services
  hosts: all
  tasks:
  - name: install packages (yum-utils)
    yum:
      name: yum-utils
      state: present
  - name: add docker engine repository
    get_url:
      url: https://download.docker.com/linux/centos/docker-ce.repo
      dest: /etc/yum.repos.d/docker.repo
      mode: 0644
  - name: download docker engine
    yum:
      name: docker-ce
      state: present
  - name: start and enable docker service
    service:
      name: docker
      enabled: yes
      state: started
  - name: install pip packages
    pip:
      name: docker
      state: present
  - name: docker login
    docker_login:
      username: "{{ username }}"
      password: "{{ password }}"
  - name: download image and start container
    docker_container:
      name: sharks-app-{{ BUILD_ID }}
      image: rufatzakirov/sharks:v{{ BUILD_ID }}
      ports:
        - "8888:8080"
