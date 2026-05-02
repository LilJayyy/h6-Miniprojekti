# h6-Miniprojekti

Tekijät: Samuli Toropainen, Lilja Sharifi, Andres Kimi Nyrhi

* [a) Userforge TSN asennus](#a-Userforge-tsn-asennus)
* [b) Miten se toimii](b-miten-se-toimii)
* [c) Lisenssi](#c-Lisenssi)


# a) Userforge TSN asennus

Userforge TSN vaatii toimiakseen Ansiblen asennuksen. 

Ansiblen saa asennettua linuxissa alla olevalla komennolla:

````bash
sudo apt-get update
sudo apt-get install -y ansible
````

Komennon syöttämisen jälkeen käyttäjän tulee syöttää `sudo-salasana + enter.`

## Ansible Asennuksen tarkistaminen

Aloitettiin Ansiblen-version tarkistamisella. Lähtökohtana tilanne, jossa Ansible on jo asennettuna.

Version tarkistaminen
````bash
ansible --version
````

* Uusin versio oli asennettuna. 

![1](images/1.png)

## Git clone 

**Varaston (repon) kloonaaminen etenee seuraavasti:**

Kloonataan repo
````bash
git clone https://github.com/LilJayyy/h6-Miniprojekti.git
````

Repon sisälle
````bash
cd h6-Miniprojekti
````

Tarkistetaan mitä sisällä tiedostoissa
````bash
ls
````

![2](images/2.png)

_Repon sisältö_

## Users.yml tiedoston sisällön luominen

Lähdetään etenemään avaamalla tekstieditori micro jolla pääsee luomaan sisällön tiedostolle

### Micro editorin asennus jos sitä ei löydy entuudestaan

Halutessasi voit käyttää myös nano editoria.

````
sudo apt install -y micro
```` 

Avataan editori
````bash
micro users.yml
````

Sisällöksi alla oleva eli käyttäjien nimet:

````
users:
  - name: matti
  - name: liisa
  - name: maija
````
Tallennetaan tiedot `ctrl + S` ja poistutaan `ctrl + Q`

### Tarkistetaan onnistuiko sisällön luonti
Katsotaan tiedoston sisälle

````bash
cat users.yml
````

![3](images/2.png)

_Users.yml sisältö_

## Playbook.yml tiedoston sisällön luominen
Playbookille sisältö yhdellä käyttäjällä

````bash
micro playbook.yml
````

Sisällöksi playbook.yml tekstitiedostoon:

````bash
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

  `hosts` kertoo ajetaanko paikallisesti, tässä tapauksessa kyllä eli `localhost`
  
  `become: yes` Sudo-oikeuksilla
  
  `ansible.builtin.user` tällä moduulilla käyttäjä luodaan
  
  `state: present` tarkistaa olemassaolon käyttäjän osalta


![4](images/4.png)

_Playbook.yml sisältö_

````bash
cat playbook.yml
````

![5](images/5.png)

_Sisältö oikein playbook.yml tiedoston sisällä_

## Ajetaan playbook

Potkaistaan playbook käyntiin
````bash
ansible-playbook playbook.yml --ask-become-pass
````

![7](images/7.png)

_Playbook ajettu onnistuneesti changed=1_


## Tarkistetaan onnistuiko käyttäjän Matti lisäys

````bash
cat /etc/passwd | grep matti
````

![6](images/6.png)

_Käyttäjä lisätty_

Matti löytyi järjestelmän tiedoista, eli käyttäjän lisäys on tehty onnistuneesti.

## Tarkistetaan toteutuuko idempotenssi

Potkaistaan playbook käyntiin
````bash
ansible-playbook playbook.yml --ask-become-pass
````
  
![8](images/8.png)

_Idempotenssi saavutettu sillä changed=0_ 

Käyttäjä "Matti" löytyi jo, eli mitään ei muutettu uuden playbookin ajon aikana. Idempotenssi on saavutettu.


## Laitetaan Playbook luomaan Users.yml tiedoston perusteella käyttäjät

**Välimuistutuksena** on tärkeää ymmärtää tehdä muutokset **projektikansion** sisällä eli 

* **`cd ~/h6-Miniprojekti`**  sisältä eikä kotihakemiston

Sijainnin tarkistaminen:
  ````bash
pwd
````

**Tämän tehtävänosion tarkoituksena on ottaa käyttäjälista käyttöön eli tehdä **silmukka** `Playbook.yml`:ään niin että se pystyy lukemaan `Users.yml` käyttäjien luomista varten.**

Lähdetään muokkaamaan playbookin sisältö
````bash
micro playbook.yml
````

Sisällöksi alla oleva:
````bash
---
- name: Userforge TSN manage users
  hosts: localhost
  connection: local
  become: yes


  vars_files:
    - users.yml

    
  tasks:
    - name: Fetch from users list
      ansible.builtin.user:
        name: "{{ item.name }}"
        state: present
      loop: "{{ users }}"
````

Tallennetaan: **`ctrl + S` ja perään `ctrl + Q`**

Ajetaan playbook ja katsoaan onnistuiko muutos
````bash
ansible-playbook playbook.yml --ask-become-pass
````

  `vars_files` laittaa nyt playbookin lukemaan users.yml:n

  `name: "{{ item.name }}"` tarkistaa nyt listasta ja lisää nimet

  `loop: "{{ users }}"` - lisää useammat käyttäjät

![9](images/9.png)

_Playbookin sisältö_

Mikäli tämä vaihe on suoritettu onnistuneesti on `liisa` ja `maija` -käyttäjät on nyt myös luotu `users.yml` -tiedostosta. 

![10](images/10.png)

_Myös testikäyttäjät maija ja liisa luotu onnistuneesti_ 

Tarkistetaan lopuksi käyttäjät
````bash
cat /etc/passwd | grep -E "matti|liisa|maija"
````

![11](images/11.png)

_Kaikki käyttäjät on nyt lisätty onnistuneesti_ 

### Tarkistetaan vielä idempotenssi

 Ajetaan playbook
````bash
ansible-playbook playbook.yml --ask-become-pass
````

![12](images/12.png)

_changed=0 eli mikään ei muutu_ 


--------------
KIMIN OSIO TÄHÄN 

----------------

### Yhteenveto: Mitä on nyt saavutettu

1. Avataan `users.yml` käyttäjä lista
2. Lisätään uusi käyttäjä
3. Ajetaan playbook
4. Uusi käyttäjä on syntynyt

Yksi tiedosto muokataan, yksi komento ajetaan = syntyy uusi käyttäjä. 












# b) Miten se toimii

KAIKI MUOKKAA TÄTÄ OSIOTA 

**--LILJAN OSUUS--**

Userforge TSN ohjelma hyödyntää toiminnassaan Ansiblen `ansible.builtin.user` moduulia. 

Moduulin tehtävänä on hakea käyttäjälista `users.yml` tiedostosta ja luoda sen perusteella käyttäjät. Tämä on projektimme **yksi totuus**.

Muutoksia hallitaan muokkaamalla `users.yml` -tiedostoa ja otetaan käyttöön kun playbook ajetaan. 

**Idempotenssi** saavutetaan niin, ettei muutoksia tehdä, jos käyttäjiin ei ole tehty muutoksia `users.yml` tiedostoon. 

**--LILJAN OSUUS--**



**--KIMIN OSUUS--**

**--KIMIN OSUUS--** 

# c) Lisenssi

GNU General Public License v3.0 (GPLv3). 

Lisätietoja voit tarkistaa `LICENSE`-osiosta.


# Lähteet ja linkit

**Ansible Community Documentation.** Dokumentti. _Using variables._ Luettavissa: https://docs.ansible.com/projects/ansible/latest/inventory/implicit_localhost.html/ Luettu: 30.4.2026

**Ansible Community Documentation.** Dokumentti. _Playbook variables._ Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_variables.html/ Luettu: 30.4.2026

**Ansible Community Documentation.** Dokumentti. _Connection details._ Luettavissa: https://docs.ansible.com/projects/ansible/latest/inventory_guide/connection_details.html/ Luettu: 30.4.2026

**Dhandala, N. 2026.** _How to Create Users with the Ansible user Module._ Luettavissa: https://oneuptime.com/blog/post/2026-02-21-how-to-create-users-with-the-ansible-user-module/view/ Luettu: 30.4.2026

**Karvinen, T. 2020.** Verkkosivu. Command Line Basics Revisited. Luettavissa: https://terokarvinen.com/2020/command-line-basics-revisited/ Luettu: 30.4.2026.

**Karvinen, T. 2026.** Verkkosivu. Hello Ansible Luettavissa: https://terokarvinen.com/hello-ansible/ Luettu: 30.04.2026.

**Karvinen, T. 2026.** Verkkosivu. Palvelinten hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/. Luettu: 2.5.2026.

**Michalowski, M.** Spacelift. Verkkosivu. _Ansible create user._ Luettavissa: https://spacelift.io/blog/ansible-create-user/ Luettu: 30.4.2026


