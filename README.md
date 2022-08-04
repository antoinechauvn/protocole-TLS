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

agit pour valider l'identité des entités (telles que les sites Web, les adresses e-mail, les entreprises ou les particuliers) et les lier à des clés cryptographiques par la publication de documents électroniques appelés certificats numériques.

![image](https://user-images.githubusercontent.com/83721477/182842128-e9184b04-2c86-4615-bce9-35ff32d9b669.png)

Un demandeur de certificat numérique génère un paire de clés publique et privée ainsi qu'une demande de signature de certificat (CSR)

### CSR (Certificate Signing Request)
Un CSR est un fichier texte **signé** avec la clé privée qui comprend la clé publique et d'autres informations qui seront incluses dans le certificat (par exemple, nom de domaine, organisation, adresse e-mail, etc.).

Informations contenues dans un CSR:
* Le nom du serveur (CN=);
* Le nom de l’entreprise qui génère la demande (O=) ;
* L’unité organisationnelle (OU=), sous la forme du numéro de SIREN précédé de « 0002 » ;
* La localité (L=) et la région (S=) où est situé le siège social de l’organisation ;
* Le pays (C=) sous la forme d’un code ISO à deux lettres ;
* L’adresse mail de l’intermédiaire au sein de l’entreprise (le plus souvent, c’est la personne en charge de la gestion des certificats).

Après avoir généré le CSR, le demandeur l'envoie à une autorité de certification, qui vérifie indépendamment l'exactitude des informations qu'il contient et, le cas échéant, signe le certificat avec la clé privée de l'autorité de certification et renvoie le certificat.

Un certificat PEM (extension .pem) contient un certificat encodé en base64 qui commence par -----BEGIN CERTIFICATE----- et se termine par -----END CERTIFICATE-----. Le format PEM est très courant.

### Chaîne de confiance

Une hiérarchie de certificats est utilisée pour vérifier la validité de l'émetteur d'un certificat. Cette hiérarchie est connue sous le nom de chaîne de confiance. Dans une chaîne de confiance, les certificats sont émis et signés par des certificats qui proviennent de plus haut dans la hiérarchie.

### X509

Le certificat x509 n’est donc pas tout à fait un certificat en tant que tel, mais une norme permettant de spécifier les formats des certificats à clé publique délivrés par les Autorités de Certification. Les certificats SSL/TLS sont des certificats x509

![image](https://user-images.githubusercontent.com/83721477/182935533-3477ca43-d61c-4f96-af80-eb85d3ed91e9.png)


https://www.youtube.com/watch?v=58FUQzWxs3Y
