PostureCoach - Application de Coaching Postural

Description

PostureCoach est une application web de coaching postural développée en partenariat avec Decathlon. L'idée est simple : on pose quelques questions aux utilisateurs sur leur pratique sportive, leurs objectifs et leurs éventuelles douleurs, puis on leur génère un profil personnalisé avec des recommandations de mouvements et d'équipement.

Le principe fonctionne comme ça : l'utilisateur répond à un quiz interactif qui analyse son niveau, ses sports, ses objectifs et ses points de douleur. Ensuite, l'app génère automatiquement un profil avec des exercices adaptés, des conseils de sécurité et des suggestions d'équipement qui correspondent vraiment à ses besoins.

Fonctionnalités Principales

Quiz Interactif

Le quiz est le cœur de l'application. Il fonctionne en plusieurs étapes avec des questions à choix unique ou multiple. On a une barre de progression visuelle pour que l'utilisateur sache où il en est. Toutes les réponses sont sauvegardées dans un contexte React, et à la fin, on génère automatiquement un profil utilisateur personnalisé.

Système de Recommandations

C'est là que ça devient intéressant. On a deux algorithmes de recommandation :

Pour les mouvements : L'algorithme filtre et trie les exercices selon plusieurs critères. D'abord, on exclut les exercices trop avancés si l'utilisateur est débutant. Ensuite, on priorise les mouvements liés aux sports qu'il pratique. On classe tout ça par pertinence et on retourne les 4 meilleurs résultats.

Pour l'équipement : On utilise un système de scoring basé sur des tags. Chaque produit a des tags (sports, objectifs, zones de douleur), et on calcule un score de pertinence en fonction de ce que l'utilisateur a indiqué. Plus il y a de correspondances, plus le score est élevé. On retourne aussi les 4 meilleurs résultats.

Interface Utilisateur

L'interface est moderne avec des animations fluides. On a créé des illustrations SVG personnalisées pour montrer les différentes postures. Le design est responsive (mobile-first) avec un thème inspiré des couleurs Decathlon. La navigation est intuitive avec un header qui reste en haut et un menu mobile pour les petits écrans.

Pages Principales

L'app contient 4 pages principales :
1. Page d'accueil : C'est la landing page avec une section hero, les fonctionnalités principales et des appels à l'action
2. Page Quiz : C'est là que se passe le questionnaire avec la barre de progression
3. Page Rapport : Le bilan personnalisé avec les mouvements recommandés et les conseils adaptés
4. Page Équipement : Un catalogue d'équipements avec des recommandations personnalisées selon le profil

Technologies Utilisées

Core

On utilise React 18.3 avec TypeScript pour le typage. Vite 5.4 sert de bundler et d'outil de développement - c'est rapide et agréable à utiliser. Pour la navigation, on a React Router 6.30. TanStack Query est déjà configuré même si on ne l'utilise pas encore, c'est pour préparer l'intégration future d'une API.

UI & Styling

Tailwind CSS 3.4 avec une configuration personnalisée. On utilise shadcn/ui comme bibliothèque de composants, qui est basée sur Radix UI. Radix UI nous donne des composants accessibles et sans style, ce qui est parfait pour avoir un contrôle total sur le design. Pour les icônes, on utilise Lucide React, et pour les animations, tailwindcss-animate.

State Management

On gère l'état avec React Context API. Pas besoin de Redux ici, le Context API suffit largement pour cette application. C'est plus simple et plus léger.

Autres

TypeScript 5.8 pour le typage strict, ESLint pour le linting, PostCSS pour le traitement CSS, et Autoprefixer pour la compatibilité avec les navigateurs.

Architecture et Points Techniques Remarquables

Structure du Projet

src/
├── components/        # Composants réutilisables
│   ├── ui/           # Composants shadcn/ui
│   └── ...           # Composants métier (Header, Footer, etc.)
├── context/          # Contextes React (QuizContext)
├── data/             # Données statiques (quizData.ts)
├── hooks/            # Hooks personnalisés
├── lib/              # Utilitaires (utils.ts)
├── pages/            # Pages de l'application
└── assets/           # Images et ressources statiques

Gestion d'État avec Context API

Le QuizContext gère l'état global du quiz de manière simple et efficace :

Structure de l'état :
{
  answers: Record<string, string[]>  // Réponses par question
  userProfile: UserProfile | null   // Profil généré après complétion
  setAnswer: (id, value) => void    // Mise à jour des réponses
  completeQuiz: () => void          // Génération du profil
  resetQuiz: () => void             // Réinitialisation
}

L'avantage, c'est qu'on n'a pas besoin de Redux pour cette application. Le Context API suffit amplement et c'est beaucoup plus simple à maintenir.

Algorithme de Recommandation

Pour les Mouvements (getRecommendedMovements)
1. Filtrage : Exclut les exercices avancés si l'utilisateur est débutant
2. Pertinence : Priorise les mouvements liés aux sports pratiqués
3. Tri : Classe par nombre de correspondances avec les sports
4. Limite : Retourne les 4 meilleurs résultats

Pour l'Équipement (getRecommendedGear)
1. Scoring : Calcule un score de pertinence basé sur les tags
2. Filtrage : Exclut les produits avec score 0
3. Tri : Classe par score décroissant
4. Limite : Retourne les 4 meilleurs résultats

Illustrations SVG Personnalisées

Le composant PostureIllustration génère des illustrations SVG dynamiques pour différents types de postures :
- Squat
- Pompes (pushup)
- Gainage (plank)
- Posture debout

La particularité, c'est que les SVG sont créés directement dans le code TypeScript. Ça permet un contrôle total sur les styles et les animations, et on peut facilement les adapter selon le contexte.

Configuration Tailwind Avancée

Le projet utilise un système de design tokens avec CSS variables :

--primary: 200 100% 38%;        /* Bleu Decathlon */
--accent: 160 80% 45%;          /* Vert énergique */
--gradient-hero: linear-gradient(...);
--shadow-elevated: 0 20px 50px...;

L'avantage de cette approche, c'est que ça facilite les changements de thème et ça maintient la cohérence visuelle dans toute l'application.

Animations Personnalisées

Animations CSS personnalisées pour améliorer l'UX :
- animate-slide-up : Apparition depuis le bas
- animate-fade-in : Fondu d'apparition
- Délais d'animation progressifs pour les listes

Difficultés Potentielles pour un Développeur

Voici les points qui pourraient poser problème si vous reprenez ce projet ou si vous voulez le comprendre en profondeur.

1. Configuration Initiale Complexe

La configuration Tailwind avec les CSS variables et les alias de chemins peut être intimidante au début. Si quelque chose ne fonctionne pas, vérifiez dans cet ordre :
- Que tailwind.config.ts est correctement configuré
- Que vite.config.ts définit bien l'alias @ vers ./src
- Que index.css importe bien toutes les variables CSS

2. Comprendre le Flux de Données du Quiz

Le flux entre le quiz, le contexte, et la génération du profil peut être confus au premier abord. Voici comment ça fonctionne :
1. L'utilisateur répond aux questions → setAnswer() met à jour le contexte
2. À la fin du quiz → completeQuiz() transforme les réponses en UserProfile
3. Le profil est utilisé pour générer les recommandations

Mon conseil : lisez QuizContext.tsx en premier, ça vous donnera une bonne vue d'ensemble de la structure.

3. Algorithme de Recommandation

Les fonctions getRecommendedMovements et getRecommendedGear peuvent sembler complexes, mais elles suivent une logique assez simple :
- Les mouvements sont filtrés par niveau ET par sports
- L'équipement utilise un système de tags et de scoring
- Les deux fonctions retournent toujours 4 résultats maximum

Le mieux pour comprendre, c'est de tester avec différents profils et de voir ce qui ressort.

4. Gestion des Types TypeScript

Les interfaces imbriquées (QuizQuestion, MovementInstruction, UserProfile) peuvent être difficiles à suivre. Tous les types sont définis dans src/data/quizData.ts - c'est votre fichier de référence si vous avez besoin de comprendre la structure des données.

5. Composants shadcn/ui

Comprendre comment utiliser les composants shadcn/ui avec leurs variants peut prendre un peu de temps. Points importants à retenir :
- Les composants sont dans src/components/ui/
- Ils utilisent cn() (de lib/utils.ts) pour fusionner les classes Tailwind
- Ils supportent des variants via class-variance-authority

6. Illustrations SVG

Créer de nouvelles illustrations SVG peut être fastidieux. Utilisez PostureIllustration.tsx comme référence. Les SVG sont assez simples et utilisent les classes Tailwind pour les couleurs, donc c'est facile à adapter.

7. Responsive Design

Gérer le responsive avec Tailwind peut être complexe si vous n'êtes pas habitué. L'approche utilisée ici :
- Mobile-first : les styles de base sont pour mobile
- Breakpoints : md:, lg: pour les écrans plus grands
- Grid responsive : grid-cols-1 md:grid-cols-2 lg:grid-cols-4

8. Navigation et Protection des Routes

La page /report nécessite un profil utilisateur. Actuellement, on fait une redirection via useEffect si pas de profil. Pour une vraie protection, il faudrait créer un composant ProtectedRoute, mais pour l'instant cette solution fonctionne bien.

9. Gestion des Animations

Coordonner les animations avec les délais peut être délicat. On utilise style={{ animationDelay: '...' }} pour créer des effets en cascade. C'est simple mais il faut faire attention aux valeurs pour que ça reste fluide.

10. Intégration Future d'API

L'application est actuellement statique, mais TanStack Query est déjà configuré. Le QueryClient est prêt dans App.tsx. Quand vous voudrez intégrer une API, il suffira d'ajouter des hooks useQuery et tout devrait fonctionner.

Installation et Démarrage

Prérequis

Vous aurez besoin de Node.js 18 ou plus. Je recommande d'utiliser nvm (https://github.com/nvm-sh/nvm) pour gérer les versions de Node. npm ou yarn fonctionnent tous les deux.

Installation

# Cloner le repository
git clone <url-du-repo>

# Installer les dépendances
npm install

# Démarrer le serveur de développement
npm run dev

L'application sera accessible sur http://localhost:8080

Scripts Disponibles

npm run dev          # Démarrage du serveur de développement
npm run build        # Build de production
npm run build:dev    # Build en mode développement
npm run preview      # Prévisualisation du build de production
npm run lint         # Vérification du code avec ESLint

Structure des Fichiers Clés

src/data/quizData.ts

C'est le fichier qui contient toutes les données statiques. Vous y trouverez les questions du quiz, les instructions de mouvements, et les équipements recommandés. C'est aussi là que se trouvent les fonctions getRecommendedMovements() et getRecommendedGear() qui font le travail de recommandation.

src/context/QuizContext.tsx

C'est là qu'on gère l'état global du quiz. Le fichier exporte QuizProvider (le provider React) et useQuiz() (le hook pour accéder au contexte). C'est un bon point de départ pour comprendre comment l'état est géré.

src/pages/

Les pages principales de l'application :
- Index.tsx : La page d'accueil
- Quiz.tsx : La page du questionnaire
- Report.tsx : La page du bilan personnalisé
- Gear.tsx : La page de l'équipement
- NotFound.tsx : La page 404

src/components/

Les composants réutilisables :
- Header.tsx : La navigation principale
- Footer.tsx : Le pied de page
- QuizQuestion.tsx : Le composant qui affiche une question
- QuizProgress.tsx : La barre de progression
- MovementCard.tsx : La carte qui affiche un exercice
- SportProfileCard.tsx : La carte qui affiche le profil
- PostureIllustration.tsx : Les illustrations SVG
- ui/ : Tous les composants shadcn/ui

Personnalisation

Modifier les Couleurs

Pour changer les couleurs, éditez src/index.css et modifiez les variables CSS. Par exemple :
--primary: 200 100% 38%;  /* Modifier cette valeur */

Toutes les couleurs utilisent ce système de variables, donc un changement ici se répercutera partout.

Ajouter des Questions

Pour ajouter une question au quiz, modifiez src/data/quizData.ts :
export const quizQuestions: QuizQuestion[] = [
  // Ajouter une nouvelle question ici
]

Il suffit de suivre le format des questions existantes.

Ajouter des Mouvements

Pour ajouter un nouveau mouvement, ajoutez-le dans le tableau movementInstructions dans src/data/quizData.ts. Assurez-vous de bien remplir tous les champs (description, posture correcte, erreurs communes, conseils de sécurité, etc.).

Modifier les Recommandations

Si vous voulez changer la logique de recommandation, ajustez les fonctions getRecommendedMovements() et getRecommendedGear() dans src/data/quizData.ts. C'est là que se trouve toute la logique de filtrage et de tri.

Améliorations Futures Possibles

Voici quelques idées d'améliorations qu'on pourrait ajouter :
- Intégration d'une API backend pour sauvegarder les profils
- Système d'authentification pour que les utilisateurs puissent revenir
- Vidéos d'exercices pour mieux montrer les mouvements
- Suivi de progression dans le temps
- Mode sombre pour ceux qui préfèrent
- Internationalisation (i18n) pour d'autres langues
- Tests unitaires et E2E pour garantir la qualité

Notes de Développement

Quelques points à retenir :
- L'application est entièrement en français pour l'instant
- Les composants Radix UI sont accessibles par défaut, ce qui est bien pour l'accessibilité
- On utilise React.memo et des optimisations Tailwind pour la performance
- Les meta tags sont configurés dans index.html pour le SEO

Contribution

Ce projet a été développé avec une approche moderne et maintenable. Si vous voulez contribuer, voici ce qu'il faut garder en tête :
1. Comprendre l'architecture Context API utilisée ici
2. Respecter les conventions TypeScript établies
3. Suivre les patterns qu'on a utilisés dans les composants existants
4. Tester les recommandations avec différents profils pour s'assurer que ça fonctionne bien

---

Développé pour aider les gens à bouger mieux et sans se blesser.

