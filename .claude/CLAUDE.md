# Guide Claude Code - HoldOn App

> Ce fichier est le point d'entrée principal pour Claude Code. Il contient les instructions, le contexte et les références nécessaires pour travailler efficacement sur le projet.

## 📚 Documentation du Projet

Pour comprendre le projet en profondeur, consulte ces fichiers dans l'ordre :

1. **[README.md](../README.md)** - Vue d'ensemble du projet, stack technique, architecture
2. **[features.md](./features.md)** - Plan d'actions détaillé et roadmap de développement

### 📋 Règles & Standards

Les règles détaillées sont disponibles dans le dossier `rules/` :

- **[general-coding.md](./rules/general-coding.md)** - Standards de code, conventions, anti-patterns
- **[security-global.md](./rules/security-global.md)** - Règles de sécurité et privacy
- **[git-workflow.md](./rules/git-workflow.md)** - Workflow Git, commits, branches, PR

## 🎯 Contexte Actuel

### Où en sommes-nous ?

**Phase actuelle** : Phase 7 - Paywall & Monétisation

**État d'avancement** :
- ✅ Phases 1-6 complètes (UI, Home, Onboarding, Permissions, Overlay, Personnalisation)
- 🔄 Phase 7 en cours : Intégration RevenueCat et logique paywall
- 🔜 Phase 8 à venir : Polish & Tests finaux

Consulte [features.md](./features.md) pour le détail complet de chaque phase.

## 💻 Style de Code & Conventions

### Standards TypeScript

```typescript
// ✅ BIEN : Composant fonctionnel avec hooks
export const MyComponent = () => {
  const [state, setState] = useState<string>('');

  return <View>...</View>;
};

// ❌ ÉVITER : Classes (sauf pour services natifs)
class MyComponent extends React.Component { }
```

### Conventions de nommage

- **Variables/Fonctions** : camelCase en anglais
- **Composants** : PascalCase
- **Fichiers** : PascalCase pour composants, camelCase pour utils
- **Comments** : En français pour la logique complexe

```typescript
// ✅ Exemple de convention
const handleButtonPress = () => { // anglais
  // Logique complexe : on vérifie si l'utilisateur a dépassé la limite
  if (selectedApps.length >= FREE_APP_LIMIT) { // français
    showPaywall();
  }
};
```

### Structure des fichiers

```
/src/screens/NewScreen.tsx          # Écran principal
/src/components/ui/NewComponent.tsx # Composant UI réutilisable
/src/services/newService.ts         # Service/logique métier
/src/hooks/useNewHook.ts            # Hook personnalisé
/src/utils/newUtil.ts               # Utilitaires
/src/types/newTypes.ts              # Types TypeScript
```

## 🚫 Anti-patterns à Éviter

### 1. Over-engineering
```typescript
// ❌ ÉVITER : Abstraction prématurée
const createButtonFactory = (type: string) => (props: ButtonProps) => { ... }

// ✅ PRÉFÉRER : Simple et direct
const PrimaryButton = ({ onPress, title }: Props) => { ... }
```

### 2. Trop de dépendances externes
- Privilégier les solutions natives React Native
- N'ajouter une lib que si vraiment nécessaire

### 3. Animations complexes qui lag
- Toujours viser 60fps
- Utiliser `useNativeDriver: true` quand possible
- Tester sur device moyen (pas uniquement flagship)

### 4. Features non-essentielles
- Focus sur le MVP
- Pas de "nice to have" avant que les features core soient rock-solid

## 🎨 Design System

### Couleurs Principales

```typescript
// src/constants/colors.ts
export const COLORS = {
  // Meditation Screens
  breathing: {
    start: '#4A90E2',  // Bleu
    end: '#7B68EE',    // Violet
  },
  sounds: {
    start: '#2ECC71',  // Vert
    end: '#16A085',    // Émeraude
  },
  sensations: {
    start: '#FF7F50',  // Orange
    end: '#FF6B6B',    // Corail
  },

  // UI
  primary: '#FF6B35',     // Orange principal
  background: '#F8F9FA',  // Gris clair
  text: '#2C3E50',        // Gris foncé
  textLight: '#7F8C8D',   // Gris moyen
};
```

### Spacing System

```typescript
export const SPACING = {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
  xxl: 48,
};
```

### Typography

```typescript
export const TYPOGRAPHY = {
  title: {
    fontSize: 28,
    fontWeight: 'bold',
  },
  subtitle: {
    fontSize: 18,
    fontWeight: '600',
  },
  body: {
    fontSize: 16,
  },
  caption: {
    fontSize: 14,
  },
};
```

## 🔧 Services Critiques

### 1. PermissionsService
Gère les permissions Android (SYSTEM_ALERT_WINDOW, Accessibility)
- Localisation : `src/services/PermissionsService.ts`
- Module natif : `android/app/src/main/java/com/holdonapp/PermissionsModule.java`

### 2. OverlayService
Gère l'affichage de l'overlay méditatif
- Localisation : `src/services/OverlayService.ts`
- Dépend de : Notifee, Permissions

### 3. StorageService
Gère la persistance locale avec AsyncStorage
- Localisation : `src/services/storage.ts`
- Données stockées : apps sélectionnées, préférences, statut premium

### 4. AccessibilityService
Service natif Android qui détecte l'ouverture d'apps
- Localisation : `android/app/src/main/java/com/holdonapp/AccessibilityService.java`
- ⚠️ Point critique : performance et fiabilité

## 📱 Workflow de Développement

### 1. Avant de coder

- Lire le contexte dans ce fichier
- Consulter [features.md](./features.md) pour la phase actuelle
- Vérifier les anti-patterns à éviter

### 2. Pendant le développement

- **Priorité** : Stabilité > Features
- **Performance** : Toujours tester les animations (60fps)
- **Permissions** : Tester les cas où elles sont refusées
- **Edge cases** : Batterie faible, appels, mode avion, etc.

### 3. Avant de commit

- Tester sur device physique (pas uniquement émulateur)
- Vérifier que les animations sont fluides
- S'assurer qu'aucune régression n'a été introduite

## 🐛 Debugging Tips

### Permissions issues
```bash
# Vérifier les permissions accordées
adb shell dumpsys package com.holdonapp | grep permission

# Réinitialiser les permissions
adb shell pm reset-permissions com.holdonapp
```

### Overlay issues
```bash
# Vérifier si l'overlay est autorisé
adb shell appops get com.holdonapp SYSTEM_ALERT_WINDOW

# Logs du service d'accessibilité
adb logcat | grep AccessibilityService
```

### Performance profiling
```bash
# Activer les métriques de performance
adb shell setprop debug.hwui.profile visual_bars

# Voir les frames droppés
adb shell dumpsys gfxinfo com.holdonapp
```

## 🎯 Tâches Courantes

### Ajouter un nouvel écran

1. Créer le composant dans `/src/screens/`
2. Ajouter la route dans la navigation (Expo Router)
3. Créer les types nécessaires dans `/src/types/`
4. Tester la navigation et les transitions

### Ajouter une nouvelle animation

1. Créer le composant dans `/src/components/animation/`
2. Utiliser `react-native-reanimated` (pas Animated API)
3. Activer `useNativeDriver` si possible
4. Tester sur device physique (60fps obligatoire)

### Modifier une permission

1. Mettre à jour `AndroidManifest.xml`
2. Modifier `PermissionsService.ts`
3. Mettre à jour l'écran de permissions dans l'onboarding
4. Tester le flow complet (accordé + refusé)

## 📝 Messages de Commit

Format recommandé :
```
type(scope): description courte

Types : feat, fix, refactor, docs, style, test, chore
Scope : screen name, service name, ou component name

Exemples :
feat(meditation): add new breathing animation variant
fix(overlay): prevent overlay during phone calls
refactor(storage): simplify AsyncStorage error handling
```

## 🚀 Build & Deployment

### Development Build
```bash
# Build de développement
npx expo run:android
```

### Release Build
```bash
# Générer un APK de release
cd android
./gradlew assembleRelease
```

### Testing Build
```bash
# Build pour test interne
eas build --profile preview --platform android
```

## 🔐 Sécurité & Privacy

**Points d'attention** :
- ❌ Aucune donnée utilisateur n'est envoyée à un serveur
- ✅ Tout est stocké localement (AsyncStorage)
- ✅ Pas d'analytics tiers
- ✅ Pas de tracking
- ⚠️ Les permissions sont sensibles : toujours expliquer clairement leur utilité

## 💡 Notes Importantes

### Performance
- Les animations doivent tourner à **60fps** minimum
- Tester sur un device moyen (pas uniquement flagship)
- Profiler régulièrement avec React DevTools

### Permissions Android
- `SYSTEM_ALERT_WINDOW` : Requis pour l'overlay
- `BIND_ACCESSIBILITY_SERVICE` : Requis pour détecter l'ouverture d'apps
- `QUERY_ALL_PACKAGES` : Requis pour lister les apps installées

### RevenueCat
- One-time purchase (pas d'abonnement)
- Prix : 4,99€
- Product ID : `unlimited_apps`
- Tester en sandbox avant la prod

### Traductions
- Support FR + EN
- Fichiers : `src/constants/translations.ts`
- Détection auto de la langue de l'appareil
- Changement manuel dans Settings

## 🤝 Comment Travailler avec Claude Code

### Ce que tu peux me demander

✅ "Implémente la fonctionnalité X"
✅ "Debug ce problème"
✅ "Optimise cette animation"
✅ "Ajoute des tests pour Y"
✅ "Refactor ce code"
✅ "Explique comment fonctionne Z"

### Ce que tu dois me préciser

- **Contexte** : Sur quel écran/feature on travaille
- **Objectif** : Ce que tu veux accomplir
- **Contraintes** : Performance, compatibilité, etc.
- **Priorité** : Urgent, important, nice-to-have

### Exemple de bonne demande

> "Je veux ajouter une nouvelle variante d'écran méditatif avec un thème 'eau'.
> Il faudrait une animation de vagues, un dégradé bleu clair → bleu foncé,
> et 5 nouveaux messages sur le thème de l'eau/fluidité.
> L'animation doit être aussi performante que les 3 existantes (60fps)."

## 📖 Ressources Externes

### Documentation Officielle
- [React Native](https://reactnative.dev/)
- [Expo](https://docs.expo.dev/)
- [React Native Reanimated](https://docs.swmansion.com/react-native-reanimated/)
- [RevenueCat React Native](https://www.revenuecat.com/docs/getting-started/installation/reactnative)
- [Android Accessibility Services](https://developer.android.com/guide/topics/ui/accessibility/service)

### Outils de Debug
- [Flipper](https://fbflipper.com/) - Debug tool
- [React DevTools](https://react-devtools-tutorial.vercel.app/) - Profiling

---

**Dernière mise à jour** : 2026-01-03
**Version du projet** : Phase 7 (Paywall en cours)
