# ConvertWEB – Guide de mise en ligne

## 📁 Structure du projet

```
convertweb/
├── index.html              ← Page principale (landing page)
├── mentions-legales.html   ← Mentions légales
├── confidentialite.html    ← Politique de confidentialité RGPD
├── cookies.html            ← Politique de cookies
├── cgv.html                ← Conditions Générales de Vente
├── sitemap.xml             ← Plan du site (pour Google)
├── robots.txt              ← Instructions pour les moteurs de recherche
└── README.md               ← Ce fichier
```

---

## ⚙️ Variables à remplacer OBLIGATOIREMENT avant mise en ligne

Recherche globale dans tous les fichiers HTML pour `[VARIABLE]` et remplace chaque occurrence.

| Variable | Description | Exemple |
|---|---|---|
| `[FORME_JURIDIQUE]` | Statut juridique | SASU, EURL, Auto-entrepreneur |
| `[NUMERO_SIREN_SIRET]` | Numéro SIREN ou SIRET | 123 456 789 00012 |
| `[NUMERO_TVA]` | N° TVA intracommunautaire | FR12345678900 |
| `[MONTANT_CAPITAL]` | Capital social | 1 000 € |
| `[ADRESSE_COMPLETE]` | Adresse complète du siège | 12 rue du Commerce, 75001 Paris |
| `[VILLE]` | Ville du siège | Paris |
| `[CODE_POSTAL]` | Code postal | 75001 |
| `[ADRESSE_RUE]` | Rue et numéro | 12 rue du Commerce |
| `[TELEPHONE]` | Numéro de téléphone pro | +33 6 12 34 56 78 |
| `[EMAIL_AGENCE]` | E-mail principal | contact@convertweb.fr |
| `[EMAIL_CONTACT]` | E-mail de contact | contact@convertweb.fr |
| `[EMAIL_DPO_OU_CONTACT]` | E-mail pour droits RGPD | rgpd@convertweb.fr |
| `[NOM_DIRECTEUR_PUBLICATION]` | Nom du responsable légal | Prénom Nom |
| `[NOM_HEBERGEUR]` | Nom de l'hébergeur | OVH SAS |
| `[ADRESSE_HEBERGEUR]` | Adresse de l'hébergeur | 2 rue Kellermann, 59100 Roubaix |
| `[URL_HEBERGEUR]` | Site web de l'hébergeur | https://www.ovhcloud.com |
| `[TELEPHONE_HEBERGEUR]` | Téléphone de l'hébergeur | +33 9 72 10 10 07 |
| `[CODE_APE]` | Code APE / NAF | 6201Z |
| `[DATE_MàJ]` | Date de dernière mise à jour | 1er janvier 2025 |
| `[DATE_ENTREE_VIGUEUR]` | Date d'entrée en vigueur CGV | 1er janvier 2025 |
| `[TAUX_PENALITES]` | Taux pénalités retard paiement | 3 fois le taux légal |
| `[AUTRES_SOUS_TRAITANTS_EVENTUELS]` | Autres sous-traitants RGPD | Mailchimp, Brevo... |
| `[NOM_MEDIATEUR]` | Nom du médiateur de la consommation | CM2C |
| `[URL_MEDIATEUR]` | URL du médiateur | https://www.cm2c.net |
| `[VILLE_TRIBUNAL]` | Ville du tribunal compétent | Paris |

---

## ✅ Checklist avant mise en ligne

### 1. Contenu & Variables
- [ ] Toutes les variables `[ENTRE_CROCHETS]` remplacées
- [ ] E-mail de contact testé et fonctionnel
- [ ] Numéro de téléphone vérifié
- [ ] Adresse correcte

### 2. Légal
- [ ] SIREN/SIRET vérifié sur le site de l'INSEE
- [ ] Mentions légales relues et validées
- [ ] CGV : tarifs cohérents avec l'offre commerciale réelle
- [ ] CGV : médiateur de la consommation choisi et renseigné
- [ ] Politique de confidentialité : durées de conservation vérifiées
- [ ] Déclarer le traitement à la CNIL si nécessaire (registre des activités)

### 3. Formulaire de contact
- [ ] Brancher le formulaire sur un backend réel (Formspree, EmailJS, PHP mailer, etc.)
- [ ] Tester l'envoi du formulaire
- [ ] Vérifier que le honeypot anti-spam fonctionne
- [ ] S'assurer que le consentement RGPD est bien enregistré avec les envois

### 4. Analytics (optionnel)
- [ ] Créer une propriété Google Analytics 4 (GA4)
- [ ] Ajouter l'ID de suivi dans `index.html` (recherche `G-XXXXXXXXXX`)
- [ ] Décommenter le snippet GA4 dans la fonction `loadAnalytics()`
- [ ] Vérifier que GA4 ne se charge QUE après consentement

### 5. SEO & Technique
- [ ] Remplacer `https://www.convertweb.fr` par votre vrai domaine dans tous les fichiers
- [ ] Mettre à jour la date dans `sitemap.xml`
- [ ] Créer et ajouter une vraie image `og-image.jpg` (1200×630 px)
- [ ] Vérifier le sitemap avec Google Search Console après mise en ligne
- [ ] Soumettre le sitemap à Google Search Console
- [ ] Activer HTTPS chez votre hébergeur (certificat SSL Let's Encrypt)

### 6. Performances
- [ ] Ajouter un favicon (`favicon.ico`, `apple-touch-icon.png`)
- [ ] Compresser les images avant de les ajouter
- [ ] Tester sur mobile (vrai smartphone)
- [ ] Tester sur Chrome, Firefox, Safari
- [ ] Lancer un test Lighthouse (DevTools → Lighthouse)

### 7. En-têtes de sécurité (côté serveur)
Ajouter ces en-têtes HTTP dans la configuration serveur (`.htaccess` Apache ou config Nginx) :
```
Content-Security-Policy: default-src 'self'; script-src 'self' cdn.tailwindcss.com; style-src 'self' 'unsafe-inline' cdn.tailwindcss.com
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=(), camera=()
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

---

## 🔌 Intégration du formulaire de contact

Le formulaire utilise actuellement une simulation (timeout). Pour le rendre fonctionnel, voici les options :

### Option A – Formspree (gratuit, sans backend)
1. Créer un compte sur [formspree.io](https://formspree.io)
2. Créer un formulaire et copier votre endpoint (ex. `https://formspree.io/f/XXXXXXXX`)
3. Dans `index.html`, remplacer le bloc `setTimeout` par :
```javascript
const formData = new FormData(form);
const response = await fetch('https://formspree.io/f/XXXXXXXX', {
  method: 'POST',
  body: formData,
  headers: { 'Accept': 'application/json' }
});
if (response.ok) {
  form.style.display = 'none';
  formSuccess.classList.remove('hidden');
}
```

### Option B – EmailJS (gratuit jusqu'à 200 emails/mois)
Suivre la documentation sur [emailjs.com](https://www.emailjs.com)

### Option C – Backend PHP ou Node.js
Envoyer les données en `fetch` vers votre propre endpoint sécurisé.

---

## 📱 Fonctionnalités à activer en production

- **Google Analytics** : décommenter dans `loadAnalytics()` dans `index.html`
- **WhatsApp Business** : liens WhatsApp dans les pages clients (hors scope de ce projet)
- **Google Maps** : ajouter la clé API Google Maps pour l'embed interactif
- **Avis Google** : intégrer l'API Google Places pour les avis en temps réel

---

## 📞 Hébergeurs recommandés pour ce type de site

| Hébergeur | Pour quoi | Lien |
|---|---|---|
| OVH / o2switch | Sites HTML statiques, PHP | ovhcloud.com / o2switch.fr |
| Netlify | Sites statiques, déploiement Git | netlify.com |
| Vercel | Sites statiques, très rapide | vercel.com |
| Infomaniak | Hébergeur suisse, RGPD-friendly | infomaniak.com |

---

*Document généré par ConvertWEB – à conserver confidentiel*
