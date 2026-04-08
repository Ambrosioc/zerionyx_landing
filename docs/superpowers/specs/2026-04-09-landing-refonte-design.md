# Refonte Landing Page Zerionyx

**Date** : 2026-04-09
**Scope** : Page d'accueil + pages secondaires (fonctionnalités, tarifs, FAQ)
**Approche** : Refonte progressive, composant par composant

---

## Objectifs

1. Réécrire le copy pour un ton humain et direct (retour utilisateur : "textes trop IA")
2. Polir le design visuel existant (transitions, spacing, cohérence)
3. Corriger les bugs identifiés
4. Appliquer les mêmes améliorations aux pages secondaires

## Hors scope

- Pas de UrgencyBar
- Pas de Testimonials (pas de vrais témoignages disponibles)
- Pas de changement de tarification (prévu plus tard)
- Pas de modification du formulaire FinalCta ni de l'API

---

## 1. Réécriture du copy

### Principes
- Ton direct, comme un artisan qui parle à un autre
- Phrases courtes, pas de buzzwords ("digitalise", "révolutionne")
- "Vous" personnel, pas de formules impersonnelles
- Retrait des émojis 🚀 sur les CTA — plus professionnel
- Du concret, pas du marketing générique

### Hero — Titres rotatifs

**Titre A** : "Vos fiches papier vous coûtent 3h par semaine. On a la solution."
**Titre B** : "Fiche remplie, signée, envoyée en PDF. En 2 minutes. Depuis votre tel."
**Titre C** : "Vous êtes 1,2 million à bosser encore sur papier. Il est temps d'arrêter."
**Sous-titre** : "Remplissez vos fiches d'intervention sur le téléphone, faites signer le client sur place, et le PDF part tout seul. Pas besoin de formation, ça marche en 2 minutes."

### Problem — Titres des 6 cards

1. "Encore 20 min à chercher une fiche"
2. "Un rapport que même vous, vous relisez pas"
3. "Le client conteste, vous avez aucune preuve"
4. "Le planning ? C'est un groupe WhatsApp"
5. "Aucune idée de ce que fait l'équipe"
6. "Des bouts de papier dans tous les tiroirs"

Les descriptions sous chaque titre seront reformulées dans le même ton direct.

### Solution — 3 étapes

1. "Vous créez votre compte en 5 min"
2. "Sur le chantier, vous remplissez la fiche sur votre tel"
3. "Le PDF se génère et part au client. C'est tout."

### Offer

**Titre** : "Les 50 premiers inscrits ont 2 mois offerts."
**CTA** : "Essayer gratuitement"

### FinalCta

**Titre** : "Prêt à lâcher le papier ?"

### Pages secondaires
- Fonctionnalités : reformulation des descriptions de chaque feature dans le même ton
- Tarifs : réécriture du texte ROI en haut
- FAQ : réécriture des réponses en langage naturel

---

## 2. Améliorations visuelles

### Hero
- Transition crossfade entre les titres (opacity + transform, au lieu de display toggle)
- Timer automatique de 5s entre chaque titre
- 3 dots indicateurs sous le titre pour montrer le titre actif
- Retrait des émojis sur le CTA

### Polish CSS global
- Transitions hover cards : 0.3s ease (au lieu de 0.22s)
- Border-radius harmonisé : 12px pour les cards, 8px pour les petits éléments
- Plus d'air entre les sections : padding vertical augmenté
- Scroll reveal : stagger 50ms (au lieu de 70ms)

### Responsive
- Problem cards : grille 2 colonnes sur tablette (breakpoint ~600-900px)
- Features tabs : scroll horizontal sur mobile
- Pricing cards : swipe hint sur mobile
- FAQ accordions : touch area minimum 48px

---

## 3. Corrections de bugs

1. **Typo Features.astro** : "synchronisee" → "synchronisée"
2. **Layout.astro** : utiliser la prop `description` dans le meta tag
3. **Compteur spots** : ajouter un minimum à 43 (pas de descente infinie)
4. **Hero rotation** : ajouter le script de rotation automatique des titres (absent actuellement)

---

## 4. Composants à supprimer

- **UrgencyBar.astro** : suppression du fichier (non utilisé, non désiré)

---

## Plan d'exécution

Ordre de traitement, composant par composant :

1. Corrections de bugs (Layout, Features typo, compteur)
2. Suppression UrgencyBar
3. Global CSS polish
4. Hero (copy + crossfade + dots + timer)
5. SocialProofBar (copy si nécessaire)
6. Problem (copy rewrite)
7. Solution (copy rewrite)
8. Features (copy rewrite + tabs scroll mobile)
9. Benefits (copy rewrite)
10. Offer (copy rewrite)
11. Pricing (responsive + swipe hint)
12. FAQ homepage (copy rewrite + touch area)
13. FinalCta (copy titre uniquement)
14. Footer (nettoyage si nécessaire)
15. Nav (retrait émoji CTA)
16. Page fonctionnalités (copy + visuel)
17. Page tarifs (copy + visuel)
18. Page FAQ (copy + visuel)
