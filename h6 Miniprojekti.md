# h6-Miniprojekti

Tekijät: Samuli Toropainen, Lilja Sharifi, Andres Kimi Nyrhi

* [a) Userforge TSN](#a-Userforge-tsn)
* [e) Lisenssi](#d-lisenssi)
* [f) Lähteet ja linkit](#f-lähteet-ja-linkit)


# a) Userforge TSN

## Ansible Asennuksen tarkistaminen

Aloitettiin Ansiblen version tarkistamisella

* **`ansible --version`** - version tarkistaminen

* Uusin versio oli asennettuna. Ohjelma vaatii Ansiblen asennuksen toimiakseen.

![1](images/1.png)

_Uusin versio asennettu_

## Git clone 

Varaston (repon) kloonaaminen etenee seuraavasti:

* **`git clone https://github.com/LilJayyy/h6-Miniprojekti.git`** - kloonataan repo

* **`cd h6-Miniprojekti`** - repon sisälle

* **`ls`** - tarkistetaan mitä sisällä tiedostoissa

![2](images/2.png)

_Repon sisältö_

## Users.yml tiedoston sisällön luominen

Lähdetään etenemään avaamalla tekstieditori micro jolla pääsee luomaan sisällön tiedostolle

* **`micro users.yml`** - avataan editori

Sisällöksi alla oleva eli käyttäjien nimet:

````
users:
  - name: matti
  - name: liisa
  - name: maija
````
Tallennetaan tiedot `ctrl + S` ja poistutaan `ctrl + Q`

### Tarkistetaan onnistuiko sisällön luonti

* **`cat users.yml`** - katsotaan tiedoston sisälle

![3](images/2.png)

_Users.yml sisältö_

## Playbook.yml tiedoston sisällön luominen

* **`micro playbook.yml`** - playbookille sisältö yhdellä käyttäjällä

Sisällöksi playbook.yml tekstitiedostoon:

````
---
- name: Userforge TSN manage users
  hosts: localhost
  connection: local
  become: yes

  tasks:
    - name: create user Matti
      ansible.builtin.user:
        name: matti
        state: present
````

hosts => kertoo ajetaanko paikallisesti, tässä tapauksessa kyllä eli `localhost`
become: yes => Sudo-oikeuksilla
ansible.builtin.user => tällä moduulilla käyttäjä luodaan
state: present => tarkistaa olemassaolon käyttäjän osalta


![4](images/4.png)

_Playbook.yml sisältö_

* **`cat playbook.yml`** - katsotaan tiedoston sisälle

![5](images/5.png)

_Sisältö oikein playbook.yml tiedoston sisällä_

## Ajetaan playbook

* **`ansible-playbook playbook.yml --ask-become-pass`** - potkaistaan playbook käyntiin

![7](images/7.png)

_Playbook ajettu onnistuneesti changed=1_


## Tarkistetaan vielä onnistuiko käyttäjän Matti lisäys

* **`cat /etc/passwd | grep matti`** -

![6](images/6.png)

_Käyttäjä lisätty_

Matti löytyi järjestelmän tiedoista, eli käyttäjän lisäys on tehty onnistuneesti.

## Tarkistetaan onnistuuko idempotenssi

* **`ansible-playbook playbook.yml --ask-become-pass`** - potkaistaan playbook käyntiin

  
![6](images/6.png)
_Idempotenssi saavutettu sillä changed=0_ 

Käyttäjä "Matti" löytyi jo, eli mitään ei muutettu uuden playbookin ajon aikana. Idempotenssi on saavutettu.


## 

# b) Asennus














# c) Käyttöönotto














# d) Miten se toimii



# e) Lisenssi



# f) Lähteet ja linkit

Ansible Community Documentation. Dokumentti. _Using variables._ Luettavissa: https://docs.ansible.com/projects/ansible/latest/inventory/implicit_localhost.html/ Luettu: 30.4.2026

Ansible Community Documentation. Dokumentti. _Playbook variables._ Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_variables.html/ Luettu: 30.4.2026

Ansible Community Documentation. Dokumentti. _Connection details._ Luettavissa: https://docs.ansible.com/projects/ansible/latest/inventory_guide/connection_details.html/ Luettu: 30.4.2026

Michalowski, M. Spacelift. Verkkosivu. _Ansible create user._ Luettavissa: https://spacelift.io/blog/ansible-create-user/ Luettu: 30.4.2026

Dhandala, N. 2026. _How to Create Users with the Ansible user Module._ Luettavissa: https://oneuptime.com/blog/post/2026-02-21-how-to-create-users-with-the-ansible-user-module/view/ Luettu: 30.4.2026
