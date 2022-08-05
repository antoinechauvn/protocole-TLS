# tls
Découverte du protocole TLS

https://www.cloudflare.com/fr-fr/learning/ssl/what-happens-in-a-tls-handshake/

* https://www.youtube.com/watch?v=P6brMpIZaOo
* https://www.youtube.com/watch?v=pRmwrp_99fA

![CLIENT SSL](https://user-images.githubusercontent.com/83721477/183156943-26aa9044-1772-44b8-81bf-dce81dfa953a.svg)

#### Client Hello:
* `Version`: numéro de version du protocole TLS que le client souhaite utiliser pour communiquer avec le serveur. Il s'agit de la version la plus élevée prise en charge par le client.
* `Client Random`: nombre pseudo-aléatoire de 32 octets utilisé pour calculer le secret principal (utilisé dans la création de la clé de chiffrement).
* `Session Identifier`: un numéro unique utilisé par le client pour identifier une session.
* `Compression Methods`: c'est la méthode qui va être utilisée pour compresser les paquets SSL. En utilisant la compression, nous pouvons réduire l'utilisation de la bande passante et, par conséquent, accélérer les vitesses de transfert.
* `Cipher Suites`: chaque suite de chiffrement contient un algorithme cryptographique pour chacune des tâches suivantes : 
  * échange de clés, 
  * authentification, 
  * chiffrement en masse (données)
  * authentification des messages. 
  
  Le client envoie une liste de toutes les suites de chiffrement qu'il prend en charge par ordre de préférence. Cela signifie que le client préférerait idéalement que la connexion soit établie à l'aide de la première suite de chiffrement envoyée.
* `Extensions`: Le client peut demander des fonctionnalités supplémentaires pour la connexion. Cela peut être fait via des extensions telles que des groupes pris en charge pour la cryptographie à courbe elliptique, des formats de points pour la cryptographie à courbe elliptique, des algorithmes de signature, etc. Si le serveur ne peut pas fournir la fonctionnalité supplémentaire, le client peut interrompre la prise de contact si nécessaire.

#### Cipher Suites Exemple:
`TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256`

* `TLS` est le protocole utilisé
* `ECDHE` est l'algorithme d'échange de clés (courbe elliptique Diffie–Hellman)
* `ECDSA` est l'algorithme d'authentification (Elliptic Curve Digital Signature Algorithm)
* `AES_128_GCM` est l'algorithme de chiffrement des données (Advanced Encryption Standard 128 bit Galois/Counter Mode)
* `SHA256` est l'algorithme Message Authentication Code (MAC) (Secure Hash Algorithm 256 bit)

<hr>

Une fois que le serveur a reçu le Client Hello , il répond par un Server Hello . Un Server Hello peut soit contenir des options sélectionnées (parmi celles proposées lors de Client Hello ), soit être un message d'échec de prise de contact.

#### Server Hello:
* `Server Version`: La version la plus élevée du protocole TLS prise en charge par le serveur qui est également prise en charge par le client.
* `Server Random`: nombre pseudo-aléatoire de 32 octets utilisé pour générer le Master Secret.
* `Session Identifier`: numéro unique permettant d'identifier la session pour la connexion correspondante avec le client. Si l'ID de session dans le message d'accueil du client n'est pas vide, le serveur trouvera une correspondance dans le cache de session. Si une correspondance est trouvée et que le serveur souhaite utiliser le même état de session, il renvoie le même ID que celui envoyé par le client. Si le serveur ne veut pas reprendre la même session, alors un nouvel ID est généré. Le serveur peut également envoyer un ID vide, indiquant que la session ne peut pas être reprise.
* `Cipher Suite`: la suite de chiffrement la plus puissante prise en charge par le serveur et le client. S'il n'y a pas de suite de chiffrement prise en charge, une alerte d'échec d'établissement de liaison est créée.
* `Compression Method`: L'algorithme de compression convenu à la fois par le serveur et le client. Ceci est facultatif.

#### Certificate (Optionnel):
Le serveur envoie au client un certificat ou une chaîne de certificats. Une chaîne de certificats commence généralement par le certificat de clé publique du serveur et se termine par le certificat racine de l'autorité de certification. Ce message est facultatif, mais est utilisé chaque fois que l'authentification du serveur est requise.

Le certificat contiendra:
* le nom d'hôte du serveur
* la clé publique utilisée par ce serveur
* la preuve d'un tiers de confiance que le propriétaire de ce nom d'hôte détient la clé privée de cette clé publique

#### Server Key Exchange (Optionnel):
Le serveur envoie au client un message d'échange de clé de serveur si les informations de clé publique du certificat ne sont pas suffisantes pour l'échange de clé. Par exemple, dans les suites de chiffrement basées sur Diffie-Hellman (DH), ce message contient la clé publique DH du serveur.


#### Certificate request (Optionnel):
Si le serveur doit authentifier le client, il envoie au client une demande de certificat. Ce message est rarement envoyé.

#### Server Hello Done:
Le serveur l'envoie au client pour confirmer que le message Server Hello est terminé.

<hr>

#### Certificate (Optionnel):
Si le serveur demande un certificat au client, le client envoie sa chaîne de certificats, tout comme le serveur l'a fait précédemment. Seules quelques serveurs demandent un certificat au client.

#### Client key exchange:
Le message Client Key Exchange est envoyé juste après la réception du Server Hello Done du serveur. Si le serveur demande un certificat client, le message est envoyé après cette étape. Au cours de cette étape, le client crée une `clé pré-maître` ou `pre-master key`.

Le secret pré-maître est créé par le client (la méthode de création dépend de la suite de chiffrement) puis partagé avec le serveur:
* Pour RSA, le client créer une pré-master key basée sur une chaîne d'octets aléatoire. Chiffre la pré-master key avec la clé publique du serveur (provenant du certificat) et l'envoie au serveur
* Pour les suites de chiffrement basées sur Diffie-Hellman, ce message contient la clé publique Diffie-Hellman du client.

Le message contiendra:

`Version`: La version du protocole du client dont le serveur vérifie si elle correspond au message hello du client d'origine.

`pre-master key`: UNIQUEMENT SI RSA

`pubkey`: clé publique Diffie-Hellman du client. UNIQUEMENT SI DIFFIE-HELLMAN

#### Certificate verify (Optionnel):
Ce message est envoyé par le client lorsque le client présente un certificat comme expliqué précédemment. Son but est de permettre au serveur de terminer le processus d'authentification du client. Lorsque ce message est utilisé, le client envoie des informations qu'il signe numériquement à l'aide d'une fonction de hachage cryptographique. Lorsque le serveur déchiffre ces informations avec la clé publique du client, le serveur est en mesure d'authentifier le client.


#### Génération du Master Secret ou Maître Secret
Une fois que le serveur a reçu la clé secrète `pré-master key`, il utilise sa clé privée pour la déchiffrer. Maintenant, le client et le serveur calculent la clé secrète principale sur la base de valeurs aléatoires échangées précédemment (Client Random et Server Random) à l'aide d'une fonction pseudo-aléatoire (PRF). Une PRF est une fonction utilisée pour générer des quantités arbitraires de données pseudo-aléatoires.

`master_secret = PRF(pre_master_secret, "master secret", ClientHello.random + ServerHello.random) [0..47];`

La clé secrète principale, d'une longueur de 48 octets, sera ensuite utilisée à la fois par le client et le serveur pour chiffrer symétriquement les données pour le reste de la communication.

Le client et le serveur créent un ensemble de 3 clés : 
* client_write_MAC_key : Authentification et contrôle d'intégrité
* server_write_MAC_key : Authentification et contrôle d'intégrité
* client_write_key : Chiffrement des messages à l'aide d'une clé symétrique
* server_write_key : Chiffrement des messages à l'aide d'une clé symétrique
* client_write_IV : Vecteur d'initialisation utilisé par certains chiffrements AHEAD
* server_write_IV : Initialisation Vecteur utilisé par certains chiffrements AHEAD

secret partagé / clé de session

#### Client Change Cipher Spec
À ce stade, le client est prêt à basculer vers un environnement sécurisé et crypté. Le protocole Change Cipher Spec est utilisé pour modifier le cryptage. Toutes les données envoyées par le client seront désormais cryptées à l'aide de la clé partagée symétrique.

#### Finished
Le dernier message du processus de prise de contact du client signifie que la prise de contact est terminée. C'est aussi le premier message crypté de la connexion sécurisée.

<hr>

#### Server Change Cipher Spec
Le serveur est également prêt à basculer vers un environnement crypté. Toutes les données envoyées par le serveur seront désormais cryptées à l'aide de la clé partagée symétrique.

#### Finished
Le dernier message du processus de poignée de main du serveur (envoyé crypté) signifie que la poignée de main est terminée.

#### Close Messages:
À la fin de la connexion, chaque côté envoie une alerte close_notify pour informer que la connexion est fermée.

<hr>

![image](https://user-images.githubusercontent.com/83721477/183220478-4aa91e60-30e9-4211-86ed-878300d2d8f0.png)

S = Pré-master key
K = Master Key

<hr>

## TLS 1.3
![TLS13](https://user-images.githubusercontent.com/83721477/152690010-0a2b1b29-eee1-4c53-851a-d9f8341b214c.png)


## CA
Une autorité de certification (CA), est une entreprise ou une organisation qui a pour but de:

* Délivrer des certificats
* Revoquer de certificats
* Renouveller des certificats

Un certificat numérique fournit :

* Authentification, en servant de justificatif d'identité pour valider l'identité de l'entité à laquelle il est délivré.
* Chiffrement, pour une communication sécurisée sur des réseaux non sécurisés comme Internet.
* Intégrité de documents signé avec le certificat afin qu'ils ne puissent pas être modifiés par un tiers en transit.

## Certificat Racine
Les navigateurs et les appareils décident de faire confiance à une AC en acceptant son certificat racine dans leur magasin de certificats racine, une base de données contenant une liste d'AC autorisées qui a été pré-installée dans le navigateur ou sur l'appareil. Windows, Apple et Mozilla (Firefox) ont tous implémenté un magasin de certificats racine dans leur système et la majorité des fabricants d'appareils mobiles ont également leur propre magasin de certificats racine.

![image](https://user-images.githubusercontent.com/83721477/182940532-3833f66d-87e5-4241-85b0-d5adcf421f74.png)

## 

![image](https://user-images.githubusercontent.com/83721477/182842128-e9184b04-2c86-4615-bce9-35ff32d9b669.png)

1. **Un demandeur de certificat numérique génère une paire de clés publique et privée ainsi qu'une demande de signature de certificat (CSR)**
2. **Après avoir généré le CSR, le demandeur l'envoie à une autorité de certification, qui vérifie indépendamment l'exactitude des informations qu'il contient**
3. **Le CA signe le certificat avec la clé privée de l'autorité de certification et renvoie le certificat.**

### CSR (Certificate Signing Request)
Un CSR est un fichier texte **signé** avec la clé privée qui comprend la clé publique et d'autres informations qui seront incluses dans le certificat (par exemple, nom de domaine, organisation, adresse e-mail, etc.).

Informations contenues dans un CSR:
* Le nom du serveur (CN=);
* Le nom de l’entreprise qui génère la demande (O=) ;
* L’unité organisationnelle (OU=), sous la forme du numéro de SIREN précédé de « 0002 » ;
* La localité (L=) et la région (S=) où est situé le siège social de l’organisation ;
* Le pays (C=) sous la forme d’un code ISO à deux lettres ;
* L’adresse mail de l’intermédiaire au sein de l’entreprise (le plus souvent, c’est la personne en charge de la gestion des certificats).

Un certificat PEM (extension .pem) contient un certificat encodé en base64 qui commence par -----BEGIN CERTIFICATE----- et se termine par -----END CERTIFICATE-----. Le format PEM est très courant.

### Chaîne de confiance

Une hiérarchie de certificats est utilisée pour vérifier la validité de l'émetteur d'un certificat. Cette hiérarchie est connue sous le nom de chaîne de confiance. Dans une chaîne de confiance, les certificats sont émis et signés par des certificats qui proviennent d'un certificat racine.

![image](https://user-images.githubusercontent.com/83721477/182942834-15709cac-3ac7-45f5-a2c4-92bf17102042.png)


### Certificat Intermédiaire
Les certificats intermédiaires font partie importante de la chaîne de confiance, ils permettent d'établir une liaison SSL avec les certificats racines de confiance présents dans les systèmes d'exploitation et les navigateurs.<br>
**Un certificat de client n'est pas émis directement par le certificat racine de l'AC, mais par le certificat intermédiaire.**

### X509

Le certificat x509 n’est donc pas tout à fait un certificat en tant que tel, mais une norme permettant de spécifier les formats des certificats à clé publique délivrés par les Autorités de Certification. Les certificats SSL/TLS sont des certificats x509

![image](https://user-images.githubusercontent.com/83721477/182935533-3477ca43-d61c-4f96-af80-eb85d3ed91e9.png)


https://www.youtube.com/watch?v=58FUQzWxs3Y

### Vérification d'un certificat

Lorsque vous visitez un site Web HTTPS, votre navigateur vérifie que la chaîne de confiance présentée par le serveur lors de la poignée de main TLS se termine à l'un des certificats racine de confiance pré-installé dans votre navigateur ou Système d'exploitation
