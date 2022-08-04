# tls
Découverte du protocole TLS

## TLS 1.2
![TLS12](https://user-images.githubusercontent.com/83721477/152689994-9ec7f1bd-f29d-4ab0-a09d-c60bb347fc97.png)

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

### Certificat Intermédiaire
Les certificats intermédiaires font partie importante de la chaîne de confiance, ils permettent d'établir une liaison SSL avec les certificats racines de confiance présents dans les systèmes d'exploitation et les navigateurs.<br>
**Un certificat de client n'est pas émis directement par le certificat racine de l'AC, mais par le certificat intermédiaire.**

### X509

Le certificat x509 n’est donc pas tout à fait un certificat en tant que tel, mais une norme permettant de spécifier les formats des certificats à clé publique délivrés par les Autorités de Certification. Les certificats SSL/TLS sont des certificats x509

![image](https://user-images.githubusercontent.com/83721477/182935533-3477ca43-d61c-4f96-af80-eb85d3ed91e9.png)


https://www.youtube.com/watch?v=58FUQzWxs3Y
