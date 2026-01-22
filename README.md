# HoldOn - Pause. Breathe. Choose.

> Une application Android de bien-être numérique qui aide à reprendre le contrôle de son attention

## Vue d'ensemble

HoldOn intercepte l'ouverture d'applications addictives (réseaux sociaux, etc.) et affiche un écran méditatif de quelques secondes. Cela permet à l'utilisateur de :
1. Se reconnecter à l'instant présent via une micro-méditation
2. Décider consciemment s'il souhaite ouvrir l'application ou y renoncer

### Différenciation

- **Approche bienveillante** (pleine conscience) vs punitive (blocker)
- **Design apaisant et minimaliste**
- **Messages courts et percutants**
- Focus sur la **qualité** plutôt que la quantité de features

## Stack Technique

- **Framework** : React Native + Expo
- **Langage** : TypeScript
- **Navigation** : Expo Router
- **Animations** : React Native Reanimated 3
- **Stockage** : AsyncStorage
- **Paiements** : RevenueCat (one-time purchase 9,99€)

## Écrans Principaux

### 1. Onboarding (3 écrans)
- **Welcome** : Logo + titre + introduction
- **Concept** : Explication du problème de l'attention et de la solution
- **Permissions** : Demande des permissions Accessibilité et Overlay avec explications sécurité

### 2. Home Screen
- Liste des applications ciblées (max 2 en gratuit, illimité en premium)
- Bouton d'ajout d'application
- Compteur de pauses effectuées
- Accès aux paramètres

### 3. App Selection Screen
- Liste de toutes les applications installées
- Barre de recherche
- Durée d'utilisation par app
- Limitation à 2 apps en version gratuite

### 4. Meditation Overlay Screen

**3 types d'écrans méditatifs** (rotation aléatoire) :

#### Type 1 : Respiration
- Animation de cercle qui pulse (4s expand / 4s contract)
- Dégradé bleu → violet
- Messages : "Respire", "Inspire profondément", etc.

#### Type 2 : Sons
- Animation d'ondes concentriques
- Dégradé vert → émeraude
- Messages : "Écoute", "Identifie 3 sons distincts", etc.

#### Type 3 : Sensations
- Animation de particules flottantes
- Dégradé orange → corail
- Messages : "Sens", "Où sont tes pieds ?", etc.

**Comportement** :
- Overlay full-screen affiché pendant 5-12 secondes (configurable)
- Après le timer : 2 boutons apparaissent
  - "Continuer vers [AppName]" → ouvre l'app
  - "Retour à l'instant présent" → ferme l'overlay

### 5. Personalize Screen
- Durée de la pause (5s, 8s, 12s)
- Cooldown entre ouvertures (30s)
- Types d'écrans affichés (rotation aléatoire ou écran personnalisé - premium uniquement)

### 6. Settings Screen
- Choix de la langue (Français / Anglais)
- À propos
- Autorisations
- Ressources (livres, articles)
- Feedback (email)
- Conditions de service
- Politique de confidentialité

### 7. Paywall Screen
- Achat unique à 9,99€
- Débloque : applications illimitées + personnalisation avancée + futures fonctionnalités
- Bouton "Restaurer un achat"

## Modèle de Monétisation

- **Version Gratuite** : 1 application ciblée
- **Version Premium** (9,99€ one-time) :
  - Applications illimitées
  - Personnalisation complète (écrans, durées)
  - Toutes les futures fonctionnalités
  - Pas d'abonnement

## Philosophie de Développement

1. **Stabilité** > Features
2. **Performance animations** > Complexité visuelle
3. **UX simple** > Options avancées
4. Tout en local (pas de backend)
5. Respect de la vie privée (aucune donnée collectée)
