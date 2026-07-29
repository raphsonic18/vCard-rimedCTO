# Carte de visite digitale — RIMED

Une carte de visite en une seule page HTML, sans backend. Elle affiche tes
informations de contact, propose un téléchargement `.vcf` (ajout aux contacts)
et génère un QR code qui, une fois scanné, propose directement "Ajouter aux
contacts" — sans passer par un site web.

## Fichier

- `carte-visite-rimed.html` — le fichier unique à modifier et à partager.
  Logo et bibliothèque QR sont intégrés dedans (base64 / inline), donc ce
  fichier fonctionne seul, sans autre dossier ni dépendance à copier.

## 1. Personnaliser tes informations

Ouvre `carte-visite-rimed.html` dans un éditeur de texte et cherche `CONFIG`
tout en bas du fichier (juste avant `</body>`). C'est le seul endroit à
modifier :

```js
const CONFIG = {
  firstName: "Prénom",
  lastName: "Nom",
  jobTitle: "Poste occupé",
  company: "RIMED",
  phoneIntl: "+212600000000",       // format international, sans espaces
  phoneDisplay: "+212 6 00 00 00 00",
  whatsappNumber: "212600000000",   // sans "+" ni espaces
  whatsappMessage: "Bonjour, j'ai récupéré votre contact via votre carte de visite digitale.",
  email: "contact@rimed.ma",
  website: "https://www.rimed.ma",
  websiteDisplay: "rimed.ma",
  linkedin: "https://www.linkedin.com/in/votre-profil",
  linkedinDisplay: "linkedin.com/in/votre-profil",
  address: "12 Rue Exemple, Casablanca 20000, Maroc"
};
```

Tout le reste (textes affichés, liens des boutons, contenu du `.vcf`, contenu
du QR code) est généré automatiquement à partir de ces valeurs. Tu n'as rien
d'autre à toucher.

**Points d'attention :**
- `phoneIntl` et `whatsappNumber` doivent être au format international
  (indicatif pays inclus), sans espaces ni caractères spéciaux, sinon les
  liens "Appeler" et "WhatsApp" ne fonctionneront pas.
- `address` alimente à la fois le bouton "Localisation" (ouvre Google Maps)
  et le champ adresse du fichier `.vcf` téléchargé.
- Chaque collaborateur qui veut sa propre carte doit dupliquer le fichier
  HTML et remplir son propre `CONFIG`.

## 2. Ouvrir et vérifier

Double-clique sur le fichier `.html` pour l'ouvrir dans ton navigateur. Vérifie
que :
- le nom, poste et coordonnées affichés sont corrects,
- les boutons Appeler / WhatsApp / Email / Site / LinkedIn ouvrent bien les
  bons liens,
- le bouton **"Ajouter à mes contacts"** télécharge un fichier `.vcf` correct,
- le QR code en bas s'affiche avec le logo au centre.

## 3. Récupérer le QR code à distribuer

En bas de la carte, clique sur **"Télécharger le QR"** : tu obtiens un PNG
prêt à l'emploi (`qr-contact-rimed.png`), avec le logo RIMED incrusté.

Ce QR encode directement ta fiche de contact (vCard) : quand un collègue le
scanne avec l'appareil photo de son téléphone, l'appareil lui propose
directement "Ajouter aux contacts". **Aucun hébergement ni site web n'est
nécessaire pour que ce QR fonctionne.**

Envoie ce PNG à tes collaborateurs par WhatsApp, email, Slack, ou mets-le
dans ta signature d'email.

## 4. Pourquoi la carte s'affiche en texte brut chez certains destinataires

Si tu envoies le fichier `.html` directement en pièce jointe (WhatsApp, email,
Slack...), l'appli du destinataire ne sait pas toujours l'ouvrir comme une
page web — elle le traite comme un document et peut afficher son contenu
brut au lieu de la carte visuelle. Deux solutions :

1. **Le destinataire ouvre le fichier explicitement dans un navigateur**
   ("Ouvrir avec... Chrome/Safari") plutôt que de le prévisualiser dans
   l'appli de messagerie. Ça marche, mais ce n'est pas automatique pour tout
   le monde.
2. **Héberger le fichier et envoyer un lien** (voir ci-dessous) — c'est la
   solution fiable : un lien s'ouvre toujours correctement dans un
   navigateur, sur n'importe quel appareil.

Le `.vcf` et le QR code, eux, fonctionnent déjà sans hébergement (voir
section 3) : c'est uniquement l'affichage de la jolie carte elle-même qui
demande un hébergement pour être fiable partout.

## 5. Héberger la carte (recommandé pour un lien partageable)

Héberge simplement `carte-visite-rimed.html` où tu veux :

- **Fichier statique** : [Netlify Drop](https://app.netlify.com/drop)
  (glisser-déposer le fichier, aucun compte requis, lien généré en
  quelques secondes), ou Vercel, GitHub Pages, l'hébergement web de ton
  entreprise. Aucune configuration serveur requise, c'est un fichier
  statique classique.
- Une fois en ligne, mets à jour les balises `<meta property="og:image">`
  et `<meta property="og:url">` dans le `<head>` du fichier avec l'URL
  réelle, pour que l'aperçu WhatsApp/email affiche bien ton logo et ton lien.
- Partage ensuite ce **lien** (pas le fichier) à tes collègues — c'est ce
  lien qui s'affichera correctement partout.

## Compatibilité

- Fonctionne sur mobile et desktop, dans tout navigateur moderne (Chrome,
  Safari, Firefox, Edge).
- Aucune donnée n'est envoyée à un serveur : tout se passe dans le
  navigateur de la personne qui ouvre le fichier.
- Respecte les préférences d'accessibilité (mouvement réduit) et les états
  de focus clavier.
