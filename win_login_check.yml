---
- name: Windows login kontrol
  hosts: all
  gather_facts: no
  ignore_unreachable: yes

  tasks:
    - name: WinRM login testi
      win_ping:
      register: login_result
      failed_when: false

    - name: Başarısız hostları listeye ekle
      set_fact:
        failed_hosts: "{{ failed_hosts | default([]) + [inventory_hostname] }}"
      when: login_result is failed or login_result is unreachable

- name: Mail gönder
  hosts: localhost
  gather_facts: no
  vars:
    failed_list: >-
      {{ hostvars | dict2items
         | selectattr('value.failed_hosts','defined')
         | map(attribute='value.failed_hosts')
         | list | flatten }}

  tasks:
    - name: Mail at
      mail:
        host: smtp.aptech.com.tr
        port: 25
        to: support@aptech.com.tr
        subject: "Windows Sunucu Login Hatası"
        body: "Login olamayan Windows sunucular: {{ failed_list }}"
      when: failed_list | length > 0
