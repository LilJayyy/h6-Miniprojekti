# Userforge TSN

Tämä projekti on toteutettu osana Haaga-helia ammattikorkeakoulun Palvelinten hallinta -opintojaksoa. Tämän projektin tarkoituksena on ollut tuottaa työkalu, joka ansiblen avulla luo uudet käyttäjät kotihakemistoineen ja generoi näille SSH-avaimet. Työkalu luo määritellyt käyttäjät, generoi heille SSH-avaimet ja asentaa julkiset avaimet käyttäjien `authorized_keys`‑tiedostoihin, mahdollistaen turvallisen avainpohjaisen kirjautumisen.

## Vaatimukset
- Linux-järjestelmä
- Ansible
- Sudo-oikeudet kohdekoneeseen

## Käyttö lyhyesti
- Määrittele luotavat käyttäjät muuttujatiedostoon (users.yml) ja aja Ansible-playbook (playbook.yml) hallintakoneelta. Playbook voidaan ajaa useita kertoja turvallisesti.

## Huomio
Projekti on tarkoitettu opetus- ja testikäyttöön eikä sellaisenaan tuotantoympäristöihin.

## Lisenssi
Projektissa sovelletaan 
