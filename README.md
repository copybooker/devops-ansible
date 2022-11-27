# Домашнее задание к занятию "1. Введение в Ansible"

1. Попробуйте запустить playbook на окружении из test.yml, зафиксируйте какое значение имеет факт some_fact для указанного хоста при выполнении playbook'a.  
  
  **ansible-playbook site.yml -i ./inventory/test.yml**
  
  *TASK [Print fact] **************************************************************
ok: [localhost] => {
    "msg": "12"}*
      
2. Найдите файл с переменными (group_vars) в котором задаётся найденное в первом пункте значение и поменяйте его на 'all default fact'.  
  Факт задан в файле group_vars/all/examp.yml, для его изменения отредактировал строчку some_fact  
  
  *TASK [Print fact] **************************************************************
ok: [localhost] => {
    "msg": "all default fact"}*

3. Воспользуйтесь подготовленным (используется docker) или создайте собственное окружение для проведения дальнейших испытаний.
4. Проведите запуск playbook на окружении из prod.yml. Зафиксируйте полученные значения some_fact для каждого из managed host.  
  **ansible-playbook site.yml -i ./inventory/prod.yml**
  
*  *TASK [Gathering Facts] *********************************************************
   ok: [ubuntu]
   ok: [centos7]*

*  *TASK [Print OS] ****************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}*

*  *TASK [Print fact] **************************************************************
ok: [centos7] => {
    "msg": "el"
}
ok: [ubuntu] => {
    "msg": "deb"
}*
  
5. Добавьте факты в group_vars каждой из групп хостов так, чтобы для some_fact получились следующие значения: для deb - 'deb default fact', для el - 'el default fact'.  
Отредактировал значения some_fact в файлах group_vars/deb/examp.yml и group_vars/el/examp.yml
6. Повторите запуск playbook на окружении prod.yml. Убедитесь, что выдаются корректные значения для всех хостов.  
**ansible-playbook site.yml -i ./inventory/prod.yml**  
*  *TASK [Gathering Facts] *********************************************************
   ok: [ubuntu]
   ok: [centos7]*

*  *TASK [Print OS] ****************************************************************
ok: [centos7] => {
    "msg": "CentOS"
}
ok: [ubuntu] => {
    "msg": "Ubuntu"
}*

*  *TASK [Print fact] **************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}*
7. При помощи ansible-vault зашифруйте факты в group_vars/deb и group_vars/el с паролем netology.
8. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь в работоспособности.  
**ansible-playbook site.yml -i ./inventory/prod.yml --ask-vault-pass**
9. Посмотрите при помощи ansible-doc список плагинов для подключения. Выберите подходящий для работы на control node.
10. В prod.yml добавьте новую группу хостов с именем local, в ней разместите localhost с необходимым типом подключения.  
*local:
      hosts:
        localhost:
          ansible_connection: local*
11. Запустите playbook на окружении prod.yml. При запуске ansible должен запросить у вас пароль. Убедитесь что факты some_fact для каждого из хостов определены из верных group_vars.  
Добавил в group_vars файл с указанием some_fact - local default fact  
*  *TASK [Print fact] **************************************************************
ok: [centos7] => {
    "msg": "el default fact"
}
ok: [localhost] => {
    "msg": "local default fact'"
}
ok: [ubuntu] => {
    "msg": "deb default fact"
}*

*  PLAY RECAP *********************************************************************
*  *centos7                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0*  
*  *localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0*  
*  *ubuntu                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0*
12. Заполните README.md ответами на вопросы. Сделайте git push в ветку master. В ответе отправьте ссылку на ваш открытый репозиторий с изменённым playbook и заполненным README.md.
