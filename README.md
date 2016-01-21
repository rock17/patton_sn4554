# patton_sn4554
Bonjour a tous, nous avons eu a configurer une passerelle patton S5445 sur notre asterisk (Trixbox) et ca n'a pas été facile.
D'une part on débute dans le domaine asterisk et d'autre part on trouve peut de tuto concernant cette passerelle.

voila un petit shema de notre installation


Avec les 2 TO on a eu 10 SDA.

configuration de la passerelle Patton SN4554

Cette config a été prise chez Patton ou on finalement trouvé un exemple.

Ensuite la config d'asterisk

creation des Trunks SIP
on créé 2 trunk sip parce qu'on a 2TO dans 4 canaux en tout et qu'il faut bien le distinger quelque part.
la config a été faite via l'interface de freepbx (on debute)

1ER TRUNK SIP

Réglages Généraux
Nb maximal de canaux: 2 /// ca correspond aux 2 canaux d'un T0

Règles de Composition de Sortie
prefixe de Numerotation:1 /// a la numerotation on ajoute un prefixe systematiquement ce prefixe sera enlevé apres par la passerelle patton ca lui sert a choisir le T0

Paramètres de Sortie
Nom du Trunk : patton1
details du peer:
host=192.168.12.164&dynamic
username=101
secret=12311
type=peer
context=from-trunk
insecure=port,invite
permit=192.168.12.164/255.255.255.255

Paramèmtres d'Entrée
context utilisateur: 101
details de l'utilisateur:
type=friend
secret=12311
context=from-trunk
host=dynamic
insecure=port,invite
permit=192.168.12.164/255.255.255.255

2EME TRUNK SIP

Réglages Généraux
Nb maximal de canaux: 2 /// ca correspond aux 2 canaux d'un T0

Règles de Composition de Sortie
prefixe de Numerotation:1 /// a la numerotation on ajoute un prefixe systematiquement ce prefixe sera enlevé apres par la passerelle patton ca lui sert a choisir le T0

Paramètres de Sortie
Nom du Trunk : patton2
details du peer:
host=192.168.12.164&dynamic
username=101
secret=12311
type=peer
context=from-trunk
insecure=port,invite
permit=192.168.12.164/255.255.255.255

Sur le 2EME TRUNK SIP on ne met pas de paramètre d'entrée ca ne sert a rien il ne faut qu'un seul trunk pour la passerelle patton puise s'enregistrer.

Ensuite creer une route entrante du genre tout arrive sur un seul poste pour essayer.

Et une route sortante:
Nom de la route: versPatton
Masque de numerotation: 0XXXXXXXXX
Sequence Tunk:
0 SIP/patton1
1 SIP/patton2

voila il faut tester.

Pour voir si votre patton s'est bien enregistré sur le trunk, sur l'interface du patton:
passez en mode Advanced GUI
SIP > GW_SIP > Status

vous devrez avoir ceci en bas
SIP Registration Manager: GW_SIP
--------------------------------

State: Registered

SIP Registration:
State: Registered
Registrar: 192.168.12.163
Used Registrar: 192.168.12.163
Logical Address:
Physical Address:
Expiration Time: 3600 s


On a passé le firemware en R5.6 et ca fonctionne aussi
