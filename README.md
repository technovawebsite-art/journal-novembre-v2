# Journal Novembre V2

Journal intime responsive dont les données sont chiffrées localement dans le navigateur.

## Fonctions

- plusieurs souvenirs par jour ;
- calendrier, recherche et statistiques ;
- humeurs, photos et objectifs ;
- export et import chiffrés ;
- verrouillage manuel ou automatique ;
- interface adaptée aux ordinateurs et aux iPhone.

## Lancer le projet

Le site se trouve dans le dossier `dist` et ne nécessite aucune dépendance.

```bash
cd dist
python -m http.server 8000
```

Ouvrir ensuite `http://localhost:8000`.

## Sécurité

Le coffre utilise PBKDF2-SHA256 et AES-GCM grâce à Web Crypto. Le mot de passe n'est pas enregistré et ne peut pas être récupéré. Les données du journal restent dans le stockage local du navigateur ; elles ne font pas partie du dépôt GitHub.

## Application iOS avec Capacitor

Le dépôt contient un projet iOS Capacitor utilisant l'identifiant :

```text
com.technova.journalnovembre
```

Pour resynchroniser le site avec le projet iOS :

```bash
npm ci
npx cap sync ios
```

## Codemagic

Le fichier `codemagic.yaml` contient deux workflows :

- `ios-altstore` : produit gratuitement une IPA non signée qu'AltServer peut signer et installer ;
- `ios-simulator` : construit une application pour le simulateur sans signature Apple ;
- `ios-release` : produit une IPA signée pour TestFlight ou l'App Store.

Pour utiliser `ios-release`, ajouter dans Codemagic un certificat Apple Distribution et un profil App Store correspondant à `com.technova.journalnovembre`. Un abonnement Apple Developer actif est nécessaire pour signer et distribuer l'application.

### Conservation des souvenirs avec AltStore

Le bundle ID reste toujours `com.technova.journalnovembre`. Un renouvellement AltServer actualise la signature sans supprimer le stockage de l'application : les souvenirs restent donc présents. Il ne faut pas supprimer l'application avant de la renouveler, car une désinstallation efface le stockage local. L'export chiffré intégré permet de restaurer le journal après une réinstallation.
