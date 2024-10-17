# Infrastructure Moderne : Concepts et Enjeux

L'objectif de cette formation est de comprendre les limites des technologies traditionnelles et de découvrir les solutions modernes en informatique. Elle est structurée autour de quatre piliers essentiels : **le code**, **l'environnement d'exécution**, **les données**, et **l'automatisation**.

## 1. Gestion du Code

### Enjeux
- **Historique des Modifications** : Conserver une trace des modifications apportées au code pour faciliter le suivi, la maintenance et le retour à des versions antérieures si nécessaire.
- **Collaboration Efficace** : Permettre à plusieurs contributeurs de travailler simultanément sur le même code sans conflit, en facilitant le partage et la fusion des contributions.
- **Source Unique de Vérité** : Garantir que tous les membres de l'équipe travaillent sur la version la plus récente et validée du code, évitant ainsi les divergences et les incohérences (référentiel unique).

### Solutions Traditionnelles et Modernes
Historiquement, des systèmes comme **CVS** (*Concurrent Versions System*) et **SVN** (*Subversion*) ont été largement utilisés pour la gestion de versions **centralisée**.  
Aujourd'hui, **Git** est devenu le standard. Il permet une gestion de versions **distribuée**, ce qui signifie que chaque utilisateur possède une copie complète du dépôt, et peut travailler **en local**, voire **hors ligne**, avant de **synchroniser** les changements avec un serveur distant.

### Avantages de Git
**Git** est indépendant de l'environnement et peut être utilisé localement, sur le cloud ou dans des environnements contraints.
- **Branches** : Facilite le développement parallèle en permettant la création de branches pour les nouvelles fonctionnalités ou corrections de bugs.
- **Issues** : Offre des mécanismes pour signaler des problèmes et discuter des solutions.
- **Pull Requests** ou **Merge Requests**: Permet de proposer et d'intégrer des modifications du code.


### Bonnes pratiques :
- **Code Indépendant de l'Environnement** : Écrire du code qui n'est pas lié à un IDE spécifique, un système d'exploitation ou une configuration particulière.
- **Documentation Intégrée** : Inclure la documentation directement dans le dépôt Git, idéalement au format **Markdown**, pour faciliter la maintenance et la génération de documents avec des outils comme **Quarto**.
- **Commande Git à Connaître**: git diff pour comparer les versions et comprendre les modifications apportées.

**Important** : Comprendre les principes fondamentaux des technologies utilisées (derrière le marketing) est essentiel pour une utilisation optimale (par exemple, Git repose sur un protocole de versionnement distribué). Ne pas se contenter des interfaces graphiques. 

### Portabilité et Flexibilité
Une base de code efficace est **agnostique aux données** et à l'**environnement**. Elle doit pouvoir être **instanciée** dans différents contextes sans rencontrer de problèmes de compatibilité. Le code doit pouvoir fonctionner indépendamment des données spécifiques, en utilisant par exemple des bases de données embarquées comme SQLite pour les modes dégradés.

### Outils Complémentaires :
- **Mermaid** : Permet de créer des diagrammes et des visualisations, intégré dans Markdown.
- **Pratiques Open Source** : Adopter les bonnes pratiques de l'open source pour améliorer la qualité, la réutilisabilité et la portabilité du code. Cela ne signifie pas nécessairement que les données doivent être publiques.

---

## 2. Environnement d'Exécution

### Limites des Postes de Travail Locaux
- **Sécurité** : Les postes locaux sont vulnérables aux failles de sécurité.
- **Scalabilité Limitée** : Difficulté à adapter les ressources matérielles (RAM, CPU) pour répondre à des charges plus importantes.
- **Travail en Équipe** : Un environnement local est peu adapté à la collaboration, surtout lors du passage en production. Le fait d'avoir des environnements hétérogènes complique le partage et la reproduction des configurations.

### Solutions Modernes
Pour résoudre ces limites, les traitements sont souvent **déportés sur des serveurs**, ce qui permet d'obtenir un environnement de développement plus proche de l'environnement de production.

#### Virtualisation (méthode traditionnelle)
Machines Virtuelles (VM) : Permettent d'exécuter plusieurs systèmes d'exploitation sur une seule machine physique, offrant une isolation complète.

**Limites**:
- **Partage des ressources** : Les utilisateurs partagent les mêmes ressources, ce qui peut entraîner des conflits.
- **Personnalisation limitée** : Difficile d'installer différentes versions de logiciels pour chaque utilisateur.

La **conteneurisation** avec **Docker** et l'orchestration avec **Kubernetes** ont émergé comme solutions modernes à ces problèmes. 

#### Conteneurisation 
La **conteneurisation** permet de créer des conteneurs légers qui encapsulent le code et ses dépendances, garantissant que l'application fonctionne de manière identique quel que soit l'environnement.

**Avantages**:
- **Isolation** des utilisateurs et des environnements: Chaque conteneur fonctionne indépendamment, évitant les conflits entre applications.
- **Efficacité** : Moins gourmands en ressources que les machines virtuelles.
- **Sécurité**: accrue.
- **Portabilité** : Les conteneurs peuvent être déployés sur n'importe quelle machine disposant d'un moteur de conteneur

Les conteneurs sont légers, rapides à déployer, et chaque machine peut en héberger un grand nombre. Un **moteur de conteneur** (comme Docker Engine) gère leur exécution et alloue les ressources nécessaires.

#### Orchestration avec Kubernetes
Kubernetes orchestre et surveille les conteneurs à grande échelle. Il gère automatiquement le déploiement, la mise à disposition des ressources, et la gestion des conteneurs dans un cluster de machines (ou nœuds). Il assure la haute disponibilité et la mise à l'échelle automatique. Des agents locaux (kubelets) surveillent l'état des conteneurs sur chaque machine et reportent à l'**API serveur** (intelligence centrale), qui prend les décisions de gestion des ressources.

**Composants clés de l'orchestration**:

- **API Server** : point d'entrée de toutes les interactions avec le cluster. C'est le "cerveau" de Kubernetes, où passent toutes les requêtes pour orchestrer les conteneurs.C'est avec lui que les utilisateurs et les systèmes communiquent pour créer, mettre à jour ou supprimer des conteneurs par exemple. 
- **Kubelet** : Processus responsable de l'exécution des conteneurs sur chaque machine (nœud). Le kubelet reçoit des instructions de l'API Server et veille à ce que les conteneurs soient bien exécutés, dans l'état souhaité. Il surveille aussi l'état des conteneurs et rapporte cette information à l'API Server.
- **Controller Manager et Scheduler** : Assurent le bon fonctionnement et le placement optimal des pods (groupes de conteneurs) dans le cluster.
- **Etcd** : Base de données clé-valeur distribuée utilisée pour stocker l'état du cluster.


### Survol historique
- **Années 1990** : Les utilisateurs intalle et déploient les applications directement  sur des machines physiques, ce qui génère des problèmes de compatibilité et de gestion des ressources.
- **Années 2000** : La virtualisation permet d'isoler les utilisateurs et assure une meilleure gestion des ressources.
- **Années 2010 à aujourd'hui** : La **conteneurisation** et l'**orchestration** ont remplacé la virtualisation, en isolant des processus plutôt que des systèmes d'exploitation entiers. Cela réduit les coûts et améliore la scalabilité. La gestion est simplifiée à grande échelle.


---

## 3. Gestion des Données

### Modèle Traditionnel
Autrefois, les données étaient stockées directement dans les environnements d'exécution et partagées via des réseaux privés. Cela limitait la flexibilité et la capacité de partage des données entre différents utilisateurs.

### Solution Moderne : Stockage Objet (S3)
**Concept** : Stocker les données sous forme d'objets dans des "buckets", accessibles via des API HTTP(S).

Le stockage **S3** (Amazon Simple Storage Service) permet d'accéder aux données via des **API** HTTP(S), ce qui assure que les données soient **indépendantes de l'environnement**. Les clients S3 peuvent ainsi gérer l'authentification et les droits d'accès à chaque requête.

### Avantages du Stockage Objet (S3) :
- **Neutralité** de l'environnement : Les données sont indépendantes de l'environnement d'exécution.
- **Accès via API** : Compatible avec des langages modernes comme Python ou R, les API permettent un accès rapide et sécurisé aux données.
- **Scalabilité** : Capacité quasi illimitée de stockage.
- **Accessibilité** : Données accessibles de n'importe où via Internet.

---

## 4. Automatisation et Déploiement (CI/CD)

### Enjeux Modernes
- **Rapidité** : Réduire le temps entre le développement et la mise en production.
- **Qualité et Fiabilité** : Assurer que le code déployé est testé et fonctionne comme prévu.
- **Sécurité** : Assurer la protection des services et des données en production.
- **Monitoring** : Suivre l'état de fonctionnement des services pour prévenir les pannes.
- **Interfaces Graphiques** : Fournir des outils intuitifs pour gérer les environnements de production.

### CI/CD
Les pipelines **CI/CD** (Intégration Continue et Déploiement Continu) sont devenus un standard pour automatiser les processus de développement et de déploiement. Ils garantissent des **déploiements rapides**, **sécurisés**, et **réversibles** en cas d'erreur.

#### Intégration Continue (CI)
L'idée est d'intégrer fréquemment le code dans un dépôt partagé où des builds automatisés et des tests sont exécutés.

**Avantages** :
- Détection rapide des bugs.
- Amélioration de la qualité du code.
- Réduction des conflits d'intégration.

#### Déploiement Continu (CD)
L'idée est de déployer automatiquement le code validé en production ou dans des environnements de staging.

**Avantages** :
- Déploiements plus rapides et plus fréquents.
- Réduction des risques grâce à des déploiements incrémentaux.
- Retour d'information rapide des utilisateurs finaux.
