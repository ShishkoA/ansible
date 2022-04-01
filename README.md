# newcentos

3. Используя ansible ad-hoc:
  - создать такого же пользователя на остальных машинах
  
  sudo pip3 install passlib
  
  python3 -c "from passlib.hash import sha512_crypt; import getpass; print(sha512_crypt.encrypt(getpass.getpass()))"
  
  ansible -i inventory all -m user -a 'name=linux password="$6$rounds=656000$3k1pKUYLd7m6gqEZ$zNJ7ORU4il5tN4j.glx2MyZ1sOJgZ1C1bHmsIp0ho7bE7Hn7U6SqALTJWSlKKUNenHVLOydjgsgeQ5JRFNysR."' -u root -k -b -K
  
  - подложить ему ssh-ключи
  
  ansible -i inventory all -m file -a 'path=/home/linux/.ssh group=linux owner=linux mode=0600 state=directory ' -u root -k -b -K
  
  ansible -i inventory all -m file -a 'path=/home/linux/.ssh/authorized_keys group=linux owner=linux mode=0700 state=touch' -u root -k -b -K
  
  ansible -i inventory all -m lineinfile -a 'path=/home/linux/.ssh/authorized_keys line="ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCZqo6soo83YM8s1CAgNL2zPF0EeqiF8yMi0v5atUKbliaUz4eFC68ln6OkL3i4LMeFzrE+FlTW02JydW3+3nFu9flUrpWjx8gA+qRMnzAH8687brUDQOFBJi70kCkCqBKumm8ncV6dAxoUyPZ/xk/kOWXFF65HKdMxtCLlUcw8Zj4e8zZkeItOs/hX90Ou1IaX4DUgeILBVBRvxKWc1BgIbQNfYF6+dij4/WsqNHKKsU+9Ot+B4t7yJ6V0I9CsGeG911bkSNigrQiR8yjijMgXeutVPPHlLqv5rlgenFEo06Mt8szglSLqwxXzD/hVGlu6YuWABP/oCUZ+4LPzyYo4RI4UDJ7XkQqfdM4kOVXc6+wGKui6GqxaEwoiNe4vzZVo8IwtyO2tmc8kmgPXHn5/++Ub2kv5Nhz3z0bFqQS7vcXO2fgvVnTr5mtkk6A8t6J1dp7sTzfZUVOI+w61mOLKPaa38vWea1TaJQvUJZHe6WP1jPCNtQuSC+RcO2evkXc= linux@ansible01"' -u root -k -b -K
  
  - дать возможность использовать sudo (помните о том, что редактирование /etc/sudoers не через visudo - плохая идея)
  
  ansible -i inventory all -m user -a 'name=linux shell=/bin/bash groups=wheel append=yes' -u root -k -b -K
