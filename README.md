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








---------------------------------------------------------------------------------


- name: Installing packages
  hosts: servers
  tasks:
  - name: Task 1
    yum:
      name: "{{ packages_1 }}"
    when: ansible_memfree_mb > 1000
  - name: Task 2
    yum:
      name: "{{ packages_2 }}"
    when: ansible_kernel_version == "4.18.0-372.26.1.el8_6.x86_64"
  - name: Start and enable nginx service
    service:
      name: nginx
      state: started
      enabled: yes
  - name: Open firewall port for nginx
    firewalld:
      service: http
      state: enabled
      immediate: yes
      permanent: yes
  - name: Create directory for nginx
    file:
      path: /home/ansible/web
      state: directory
  - name: index.html for /web
    template:
      src: index.html.j2
      dest: /home/ansible/web/index.html
  - name: copy true config in nginx.conf
    copy:
      src: nginx.conf
      dest: /etc/nginx/nginx.conf
      
      
      ------------------------------------------------------------
      /group_vars/servers
      
      packages_1:
        - vim
        - nano
        - redis
      packages_2:
        - bash-completion
        - nano
        - nginx
