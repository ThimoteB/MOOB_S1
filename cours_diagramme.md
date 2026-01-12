# 🏗️ UML - Conception et Modélisation

> **UML** (Unified Modeling Language) : langage graphique standardisé pour modéliser les systèmes orientés objet.

---

## 📋 Table des Matières

```mermaid
mindmap
  root((UML))
    Cycle en V
      Spécification
      Conception
      Tests
    Cas d'Utilisation
      Acteurs
      Include/Extend
    Diagrammes de Séquence
      Analyse
      Conception
    Diagrammes de Classe
      Relations
      Multiplicités
    États-Transitions
      Automates
```

---

# 📐 PARTIE 1 — CYCLE DE DÉVELOPPEMENT (CYCLE EN V)

> Chaque phase de **conception** correspond à une phase de **validation**.

```mermaid
flowchart TB
    subgraph Descente["⬇️ Conception"]
        A["📋 Spécification<br/>(MOA)"]
        B["🏗️ Conception Générale"]
        C["🔧 Conception Détaillée"]
        D["💻 Codage"]
    end
    
    subgraph Montee["⬆️ Validation"]
        E["✅ Tests Unitaires"]
        F["🔗 Tests d'Intégration"]
        G["📦 Tests Système"]
        H["🎯 Recette (MOA)"]
    end
    
    A --> B --> C --> D
    D --> E --> F --> G --> H
    
    C -.-|"valide"| E
    B -.-|"valide"| F
    A -.-|"valide"| H
```

| Phase | Responsable | Validation par |
|-------|-------------|----------------|
| Spécification | MOA (Client) | Recette finale |
| Conception Générale | MOE (Équipe) | Tests d'intégration |
| Conception Détaillée | Développeur | Tests unitaires |

---

# 🎭 PARTIE 2 — DIAGRAMMES DE CAS D'UTILISATION

> Modélise **ce que fait** le système du point de vue de l'utilisateur.

## 2.1 Structure de Base

```mermaid
flowchart LR
    subgraph Systeme["🖥️ Système"]
        UC1(("Consulter<br/>Catalogue"))
        UC2(("Commander"))
        UC3(("Payer"))
    end
    
    Actor1["🧑 Client"]
    Actor2["🏦 Banque"]
    
    Actor1 --- UC1
    Actor1 --- UC2
    Actor1 --- UC3
    UC3 --- Actor2
```

## 2.2 Relations entre Cas d'Utilisation

### 📊 Légende des Relations UML

| Relation | Notation | Signification |
|----------|----------|---------------|
| **Include** | `- - - ▷` pointillé | A **inclut obligatoirement** B |
| **Extend** | `- - - ▷` pointillé | A **peut étendre** B (optionnel) |
| **Généralisation** | `───▷` plein vide | A **est un type de** B |

### Include (Inclusion obligatoire)

> Le cas A **déclenche toujours** le cas B.

```mermaid
flowchart LR
    A(("Commander"))
    B(("S'authentifier"))
    
    A -.->|"«include»"| B
```

**Exemple** : *Commander* inclut obligatoirement *S'authentifier*.

### Extend (Extension optionnelle)

> Le cas A **peut optionnellement** déclencher B.

```mermaid
flowchart LR
    A(("Commander"))
    B(("Souscrire<br/>Assurance"))
    
    B -.->|"«extend»"| A
```

**Exemple** : *Souscrire une assurance* étend optionnellement *Commander*.

### Généralisation (Spécialisation)

> Le cas A **est une spécialisation** du cas B.

```mermaid
flowchart BT
    A(("Payer CB"))
    B(("Payer PayPal"))
    C(("Payer"))
    
    A -->|"hérite"| C
    B -->|"hérite"| C
```

**Exemple** : *Payer CB* et *Payer PayPal* sont des types de *Payer*.

---

# ⏳ PARTIE 3 — DIAGRAMMES DE SÉQUENCE

> Représente les **interactions chronologiques** entre objets/acteurs.

## 3.1 Éléments Graphiques

| Élément | Représentation | Description |
|---------|----------------|-------------|
| **Acteur** | 🧑 Bonhomme | Entité externe au système |
| **Objet** | 📦 Rectangle | Instance d'une classe |
| **Ligne de vie** | `┆` pointillé vertical | Durée de vie de l'objet |
| **Message synchrone** | `───▶` plein | Appel bloquant (attend réponse) |
| **Message retour** | `- - -▶` pointillé | Réponse à un appel |
| **Message asynchrone** | `───>` flèche ouverte | Appel non bloquant |

## 3.2 Niveau Analyse (Vue simplifiée)

> Vision "boîte noire" du système.

```mermaid
sequenceDiagram
    actor Client
    participant Systeme as 🖥️ Système
    
    Client->>Systeme: commander(produits)
    activate Systeme
    Systeme-->>Client: confirmation
    deactivate Systeme
    Systeme-->>Client: emailConfirmation
```

## 3.3 Niveau Conception (Vue détaillée)

> Interactions entre objets internes.

```mermaid
sequenceDiagram
    actor Client as 🧑 Sam
    participant UI as :InterfaceWeb
    participant Ctrl as :ControleurCommande
    participant DB as :BaseDonnées
    
    Client->>UI: commander("Livre Java")
    activate UI
    UI->>Ctrl: traiterCommande(client, produit)
    activate Ctrl
    Ctrl->>DB: verifierStock("Livre Java")
    activate DB
    DB-->>Ctrl: stockOk
    deactivate DB
    Ctrl->>DB: enregistrerCommande(...)
    DB-->>Ctrl: commandeId
    Ctrl-->>UI: CommandeConfirmée(id)
    deactivate Ctrl
    UI-->>Client: "Commande #123 confirmée"
    deactivate UI
```

## 3.4 Fragments Combinés

| Fragment | Usage |
|----------|-------|
| `alt` | Alternative (if/else) |
| `opt` | Optionnel (if sans else) |
| `loop` | Boucle |
| `par` | Parallèle |
| `break` | Sortie anticipée |

```mermaid
sequenceDiagram
    actor User
    participant Auth as :Authentification
    
    User->>Auth: login(user, pwd)
    
    alt Mot de passe correct
        Auth-->>User: session créée
    else Mot de passe incorrect
        Auth-->>User: erreur 401
    end
```


---

# ⛓️ PARTIE 4 — DIAGRAMMES DE CLASSE

> Modélise la **structure statique** : classes, attributs, méthodes et relations.

## 4.1 Structure d'une Classe

```mermaid
classDiagram
    class NomClasse {
        +attributPublic: Type
        #attributProtege: Type
        -attributPrive: Type
        __
        +methodePublique(): ReturnType
        #methodeProtegee(param: Type): void
        -methodePrive(): void
    }
```

### Symboles de Visibilité

| Symbole | Visibilité | Java |
|:-------:|------------|------|
| `+` | Public | `public` |
| `#` | Protégé | `protected` |
| `-` | Privé | `private` |
| `~` | Package | *(défaut)* |

## 4.2 Relations UML — Légende Complète

```mermaid
classDiagram
    direction TB
    
    class Parent {
        +methode(): void
    }
    class Enfant {
        +methode(): void
    }
    Parent <|-- Enfant : extends (héritage)
    
    class Interface {
        <<interface>>
        +contrat(): void
    }
    class Implementation {
        +contrat(): void
    }
    Interface <|.. Implementation : implements
    
    class Tout {
    }
    class Partie {
    }
    Tout *-- Partie : composition (partie meurt avec tout)
    
    class Conteneur {
    }
    class Element {
    }
    Conteneur o-- Element : agrégation (élément survit)
    
    class ClasseA {
    }
    class ClasseB {
    }
    ClasseA ..> ClasseB : dépendance (utilise temporairement)
    
    class ClasseC {
    }
    class ClasseD {
    }
    ClasseC -- ClasseD : association (lien permanent)
```

### Exemples concrets pour chaque relation

| Relation | Exemple concret | Explication |
|----------|-----------------|-------------|
| **Héritage** | `Animal <\|-- Chien` | Un Chien **est un** Animal |
| **Implémentation** | `Comparable <\|.. Produit` | Produit **implémente** l'interface Comparable |
| **Composition** | `Maison *-- Piece` | Une Pièce n'existe pas sans sa Maison |
| **Agrégation** | `Equipe o-- Joueur` | Un Joueur peut exister sans son Équipe |
| **Dépendance** | `Commande ..> Paiement` | Commande utilise Paiement temporairement (en paramètre) |
| **Association** | `Etudiant -- Universite` | Lien permanent entre Etudiant et Université |

### 🔑 Mémo Visuel Rapide

| Ce que vous voyez | Ce que ça signifie | Mot-clé Java |
|-------------------|-------------------|--------------|
| `───▷` trait **plein** + triangle **vide** | **Héritage** | `extends` |
| `- - -▷` trait **pointillé** + triangle **vide** | **Implémentation** | `implements` |
| `◆───` losange **plein** | **Composition** | (contient, cycle de vie lié) |
| `◇───` losange **vide** | **Agrégation** | (contient, cycle de vie indépendant) |
| `- - -▶` trait **pointillé** + flèche | **Dépendance** | (paramètre, variable locale) |
| `───▶` trait **plein** + flèche | **Association dirigée** | (attribut) |

### 🎯 Astuce pour retenir

- **Triangle** = relation de type/sous-type (héritage ou implémentation)
  - Plein = `extends` (classe concrète)
  - Pointillé = `implements` (interface)
- **Losange** = relation tout/partie
  - Plein ◆ = fort (composition)
  - Vide ◇ = faible (agrégation)

### 📊 Tableau des Relations (Normes UML)

| Relation | Flèche UML | Trait | Extrémité | Mermaid |
|----------|------------|-------|-----------|---------|
| **Association** | `───────` | Plein | Aucune | `--` |
| **Association dirigée** | `───────▶` | Plein | Flèche pleine | `-->` |
| **Dépendance** | `- - - - -▶` | Pointillé | Flèche pleine | `..>` |
| **Héritage** | `───────▷` | Plein | Triangle vide | `--|>` |
| **Implémentation** | `- - - - -▷` | Pointillé | Triangle vide | `..|>` |
| **Agrégation** | `◇───────` | Plein | Losange vide | `o--` |
| **Composition** | `◆───────` | Plein | Losange plein | `*--` |

## 4.3 Héritage (extends) — Triangle Vide + Trait Plein

```mermaid
classDiagram
    class Animal {
        <<abstract>>
        #nom: String
        +manger(): void
        +seDeplacer()* void
    }
    
    class Chien {
        +aboyer(): void
        +seDeplacer(): void
    }
    
    class Chat {
        +miauler(): void
        +seDeplacer(): void
    }
    
    Animal <|-- Chien
    Animal <|-- Chat
```

> ⚠️ **Norme UML** : Triangle **vide** (non rempli) + trait **plein**

## 4.4 Implémentation (implements) — Triangle Vide + Trait Pointillé

```mermaid
classDiagram
    class Volant {
        <<interface>>
        +voler(): void
    }
    
    class Nageur {
        <<interface>>
        +nager(): void
    }
    
    class Canard {
        +voler(): void
        +nager(): void
    }
    
    Volant <|.. Canard
    Nageur <|.. Canard
```

> ⚠️ **Norme UML** : Triangle **vide** + trait **pointillé**

## 4.5 Composition ◆ vs Agrégation ◇

```mermaid
classDiagram
    class Voiture {
        -immatriculation: String
    }
    
    class Moteur {
        -puissance: int
    }
    
    class Roue {
        -taille: int
    }
    
    class Passager {
        -nom: String
    }
    
    Voiture *-- "1" Moteur : possède
    Voiture *-- "4" Roue : possède
    Voiture o-- "0..5" Passager : transporte
```

| | Composition ◆ | Agrégation ◇ |
|--|---------------|--------------|
| **Symbole** | Losange **plein** | Losange **vide** |
| **Cycle de vie** | Partie **meurt** avec le tout | Partie **survit** sans le tout |
| **Exemple** | Voiture ◆── Moteur | Voiture ◇── Passager |

## 4.6 Multiplicités

```mermaid
classDiagram
    class Entreprise {
        +nom: String
    }
    
    class Employe {
        +nom: String
    }
    
    class Projet {
        +titre: String
    }
    
    Entreprise "1" --o "*" Employe : emploie
    Employe "*" -- "*" Projet : participe
```

| Notation | Signification |
|----------|---------------|
| `1` | Exactement un |
| `0..1` | Zéro ou un (optionnel) |
| `*` ou `0..*` | Zéro ou plusieurs |
| `1..*` | Au moins un |
| `n..m` | Entre n et m |

## 4.7 Classe d'Association

> Quand une **relation** porte ses propres **attributs**.

```mermaid
classDiagram
    class Etudiant {
        +nom: String
        +matricule: int
    }
    
    class Cours {
        +intitule: String
        +credits: int
    }
    
    class Inscription {
        <<association class>>
        +dateInscription: Date
        +note: float
    }
    
    Etudiant "*" -- "*" Cours : suit
    Inscription .. Etudiant
    Inscription .. Cours
```

## 4.8 Exemple Complet

```mermaid
classDiagram
    class Vehicule {
        <<abstract>>
        #marque: String
        +demarrer()* void
    }
    
    class Roulant {
        <<interface>>
        +rouler(): void
    }
    
    class Voiture {
        -nbPortes: int
        +demarrer(): void
        +rouler(): void
    }
    
    class Moteur {
        -cylindree: int
        +allumer(): void
    }
    
    class GPS {
        -modele: String
    }
    
    class Conducteur {
        -permis: String
    }
    
    Vehicule <|-- Voiture : extends
    Roulant <|.. Voiture : implements
    Voiture *-- "1" Moteur : contient
    Voiture o-- "0..1" GPS : équipé de
    Voiture "0..*" -- "1..*" Conducteur : conduit par
```

---

# 🔄 PARTIE 5 — DIAGRAMMES D'ÉTATS-TRANSITIONS

> Décrit le **comportement dynamique** d'un seul objet (automate).

## 5.1 Éléments Graphiques

| Élément | Symbole | Description |
|---------|:-------:|-------------|
| État initial | ● | Cercle noir plein |
| État | ⬜ arrondi | Rectangle aux coins arrondis |
| État final | ◉ | Cercle dans cercle |
| Transition | `───▶` | Flèche avec label |

## 5.2 Syntaxe des Transitions

```
événement [garde] / action
```

| Composant | Obligatoire | Description |
|-----------|:-----------:|-------------|
| Événement | ✅ | Ce qui déclenche la transition |
| Garde | ❌ | Condition entre `[crochets]` |
| Action | ❌ | Comportement après `/` |

## 5.3 Exemple : Distributeur Automatique (GAB)

```mermaid
stateDiagram-v2
    [*] --> Inactif
    
    Inactif --> CarteInseree : insererCarte
    
    CarteInseree --> Inactif : annuler / ejecterCarte
    CarteInseree --> CodeValide : saisirCode [codeOK]
    CarteInseree --> CarteInseree : saisirCode [codeKO && essais<3] / essais++
    CarteInseree --> CarteBloguee : saisirCode [essais>=3]
    
    CodeValide --> ChoixOperation : afficherMenu
    ChoixOperation --> Transaction : choisirMontant
    Transaction --> Inactif : terminer / ejecterCarte
    
    CarteBloguee --> [*] : avalerCarte
```

## 5.4 Actions Internes

| Mot-clé | Déclencheur | Exemple |
|---------|-------------|---------|
| `entry/` | À l'entrée dans l'état | `entry / nbEssais = 0` |
| `exit/` | À la sortie de l'état | `exit / sauvegarder()` |
| `do/` | Pendant tout l'état | `do / clignoter()` |

---

# 📋 RÉCAPITULATIF — NORMES UML

## Flèches des Diagrammes de Classe

| Relation | Trait | Extrémité côté cible | Mermaid |
|----------|:-----:|:--------------------:|:-------:|
| Héritage | ─── plein | ▷ triangle vide | `<\|--` |
| Implémentation | - - - pointillé | ▷ triangle vide | `<\|..` |
| Composition | ─── plein | ◆ losange plein (côté conteneur) | `*--` |
| Agrégation | ─── plein | ◇ losange vide (côté conteneur) | `o--` |
| Dépendance | - - - pointillé | ▶ flèche pleine | `..>` |
| Association dirigée | ─── plein | ▶ flèche pleine | `-->` |

## Vue d'Ensemble des Diagrammes

| Diagramme | Type de vue | Question |
|-----------|-------------|----------|
| Cas d'utilisation | Fonctionnelle | *Que fait le système ?* |
| Séquence | Dynamique | *Comment interagissent-ils ?* |
| Classe | Structurelle | *Comment est-ce organisé ?* |
| États-transitions | Comportementale | *Comment évolue l'objet ?* |

---

> **💡 Conseil** : En UML, chaque détail de flèche a un sens précis. Respectez les conventions pour une communication claire avec votre équipe !
