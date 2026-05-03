# Userforge TSN
Tämä projekti on toteutettu osana Haaga-helia ammattikorkeakoulun Palvelinten hallinta -opintojaksoa. Työkalu luo määritellyt käyttäjät ja generoi heille SSH-avainparit paikalliseen tiedostojärjestelmään.

## Vaatimukset
- Linux-järjestelmä
- Ansible
- Sudo-oikeudet kohdekoneeseen

## Käyttö lyhyesti
- Määrittele luotavat käyttäjät muuttujatiedostoon (`users.yml`)
- Aja Ansible-playbook (`playbook.yml`) hallintakoneelta. Playbook voidaan ajaa useita kertoja turvallisesti.

## Huomio
Projekti on tarkoitettu opetus- ja testikäyttöön eikä sellaisenaan tuotantoympäristöihin.

## Lisenssi
Projektissa sovelletaan GPL-3.0-lisenssiä.
