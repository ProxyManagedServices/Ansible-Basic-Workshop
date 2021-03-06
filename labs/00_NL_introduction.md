# Introductie
Welkom bij de workshop Ansible-Basic. In deze workshop gaan we een Azure VM met Linux inrichten met Ansible. Na de workshop is je Linux VM nog een aantal dagen beschikbaar, zodat je deze kunt gebruiken voor de ontwikkeling van je eigen Ansible playbooks. Je kunt er natuurlijk ook een demo mee geven aan je collega's. 

De workshop is zo opgezet, dat deze vanaf iedere plek verder afgemaakt kan worden. 

## Benodigdheden
Voor deze workshop heb je nodig:
- Laptop met een SSH client (bijvoorbeeld putty: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- Een Linux server met SSH toegang en Ansible (in deze workshop de bastion server: du-bastion.westeurope.cloudapp.azure.com)
- Sheet met account gegevens 

## Uitvoering
- Alle acties worden uitgevoerd vanuit de home directory van de SSH server (tenzij anders aangegeven).
- Instructies voor commando's worden vooraf gegaan met een prompt teken ($) en staan in een tekst blok. Dit teken is onderdeel van de opdracht prompt en hoort niet bij het commando (wie kent de dos prompt ``C:\>`` nog?). Bijvoorbeeld (Het commando in het voorbeeld is dus ``ls``):

  ``$ ls``
  
- De output wordt ook in een tekstblok weergegeven. Bijvoorbeeld:
  ```
  pi@raspberry:~ $ ls -l
  total 3
  -rw-r--r-- 1 pi pi   84 Feb 14 13:30 ansible.cfg
  -rw-r--r-- 1 pi pi  979 Feb 14 17:07 brocade.yml
  -rw-r--r-- 1 pi pi  884 Feb 14 22:07 cisco.yml
  ```

  
Volgende stap: [Lab 1 - Inventory file aanmaken](/labs/01_NL_inventory.md)
