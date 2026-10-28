# Roadmap : application PBC List

Rédigée le 28/10/2026 avec Juju (comptable). Statut : **brouillon à valider**.

## 1. Pourquoi

Lorsque le cabinet consulte les transactions d'un client dans Pennylane, de nombreux virements n'ont pas de facture. La « demande comptable » actuelle de Pennylane envoie au client une liste brute, difficile à analyser pour lui.

**Objectif :** offrir au client un parcours simple et guidé, étape par étape, pour fournir les factures fournisseurs manquantes.

## 2. Ce qui est décidé

| Sujet | Décision |
|---|---|
| Utilisateurs | Une application unique pour tous les clients du cabinet. Chaque client ne voit que ses propres demandes. |
| Accès client | Un espace client avec identifiant. |
| Présentation des demandes | Les virements sans facture sont **regroupés par fournisseur** (ex. « Amazon : 12 virements »). |
| Fourniture des factures | Pour chaque fournisseur, le client peut **déposer** ses factures (PDF, JPG, PNG) **ou** les faire suivre par e-mail. |
| Adresse e-mail | C'est l'adresse achats générée par Pennylane pour chaque dossier (`nom-du-dossier-XXXX@suppliers.pennylane.com`), affichée avec un bouton « copier ». |
| Identifiants fournisseurs | **Aucun identifiant ni mot de passe tiers n'est stocké** (ex. compte Amazon). |
| Outil final | La maquette, puis l'application, sont construites sur **Lovable**. |

### Pourquoi pas de mots de passe tiers

- RGPD, art. 5.1.f (intégrité et confidentialité), art. 25 (protection dès la conception) et art. 32 (sécurité du traitement).
- CNIL, délibération n° 2022-100 du 21 juillet 2022, recommandation relative aux mots de passe.
- En pratique, la double authentification des fournisseurs bloque souvent la connexion automatique.

### Adresse e-mail Pennylane (vérifiée)

Selon la source [Pennylane, « Importer des factures par transfert d'e-mail »](https://help.pennylane.com/fr/articles/185021-importer-des-factures-par-transfert-d-e-mail) :
- **Activation :** Paramètres > Transmission de factures > Adresses e-mail > Activer.
- **Formats :** PDF, JPG et PNG (plus de 400 × 400 pixels), en pièce jointe ou dans le corps de l'e-mail.
- **Arrivée :** les factures arrivent dans l'onglet Achats.
- **Limites :** l'adresse n'est pas modifiable et doit rester confidentielle, car tout document reçu est importé.

## 3. Parcours client (maquette)

1. **Connexion** à l'espace client.
2. **Accueil.** Un message simple, par exemple « Il nous manque 18 factures, réparties chez 4 fournisseurs », avec une barre de progression.
3. **Une carte par fournisseur.** Elle indique le nom, le nombre de virements, le montant total et un statut (à faire, en cours, terminé).
4. **Écran d'un fournisseur.** Il présente la liste des virements concernés (date, montant, libellé bancaire) et deux choix :
   - **« Je dépose mes factures »** : glisser-déposer des fichiers, un par virement ou plusieurs d'un coup ;
   - **« Je les envoie par e-mail »** : l'adresse Pennylane avec un bouton « copier », et une courte aide pour la renseigner dans le compte du fournisseur ou transférer les e-mails.
5. **Option « Je n'ai pas de facture ».** Le client peut ajouter un commentaire libre (ex. « frais personnels », « facture perdue »). *Proposition de Claude, à valider.*
6. **Fournisseur suivant**, jusqu'à la fin, puis un écran de confirmation.

## 4. Côté cabinet

- **Création des demandes** à partir des virements sans justificatif d'un client.
- **Tableau de bord par client** : statut de chaque fournisseur (« à faire », « déposé », « transféré par e-mail », « sans facture »). *Proposition par défaut, en attente de votre avis.*

## 5. Étapes (roadmap)

| Étape | Contenu | Résultat |
|---|---|---|
| **0. Cadrage** | Besoins, décisions RGPD, adresse Pennylane | Ce document (fait) |
| **1. Maquette Lovable** | Écrans du parcours client et tableau de bord cabinet, avec **données fictives** | Lien de prévisualisation à tester |
| **2. Source des données** | Comment obtenir la liste des virements sans facture : export Pennylane, liste préparée avec Claude via le MCP Pennylane, puis import dans l'app | Méthode validée sur un client test |
| **3. Comptes et sécurité** | Connexion client (Lovable / Supabase Auth), droits « un client ne voit que ses données », stockage des fichiers déposés | Espace client sécurisé |
| **4. Envoi vers Pennylane** | Les fichiers déposés sont transmis à l'adresse achats Pennylane du dossier | Factures dans l'onglet Achats |
| **5. Pilote** | Test avec 1 ou 2 clients volontaires | Retours et ajustements |
| **6. Plus tard (V2)** | Relances automatiques, lien direct vers la page « Mes factures » des principaux fournisseurs, connexion directe à Pennylane | À décider |

## 6. Points ouverts (à vérifier, rien n'est supposé)

1. **Repérer les virements sans facture.** L'outil MCP Pennylane permet de lister les transactions bancaires, mais je n'ai pas trouvé de filtre explicite « sans justificatif ». C'est à tester sur un dossier réel à l'étape 2.
2. **Lien entre l'application et Pennylane.** Le MCP Pennylane est un outil utilisé par Claude, l'application Lovable ne peut pas s'en servir directement. Une connexion automatique demanderait l'API Pennylane : disponibilité selon votre offre et coût à vérifier.
3. **Gratuité.** Lovable et Supabase ont des offres gratuites limitées (crédits, stockage). Les plafonds sont à vérifier avant le pilote.
4. **Hébergement en ligne.** Lovable héberge l'application en ligne. La maquette n'utilisera que des données fictives. Pour les vraies factures, il faudra une information RGPD aux clients et un contrat de sous-traitance (art. 28 RGPD).
5. **Suivi côté cabinet.** Votre avis est attendu sur le tableau de bord.
