# 📝 SHEETNOTE — Java POO & UML

> Fiche recto-verso pour le partiel

---

# 🅰️ RECTO — JAVA POO

## 1. Types & Références

```
Primitifs: int, double, boolean, char, byte, short, long, float
Objets: String, tableaux, classes → RÉFÉRENCES (adresses)

Passage paramètres:
- Primitif → COPIE (original intact)
- Objet    → COPIE de la référence (contenu modifiable)
```

## 2. Visibilité

| Modif | Classe | Package | Fille | Monde |
|-------|:------:|:-------:|:-----:|:-----:|
| `public` | ✓ | ✓ | ✓ | ✓ |
| `protected` | ✓ | ✓ | ✓ | ✗ |
| *(défaut)* | ✓ | ✓ | ✗ | ✗ |
| `private` | ✓ | ✗ | ✗ | ✗ |

## 3. Mots-clés Essentiels

```
static    → appartient à la CLASSE (pas à l'instance)
final     → constant / non redéfinissable / non héritable
abstract  → incomplet, doit être implémenté
this      → référence à l'instance courante
super     → référence à la classe mère
```

## 4. Héritage & Polymorphisme

```java
class Fille extends Mere { }        // Héritage
class Impl implements Interface { } // Implémentation

@Override  // Redéfinition (polymorphisme)
```

**Règles:**
- 1 seule classe mère (extends)
- Plusieurs interfaces (implements)
- Constructeur fille → `super()` en 1er

## 5. Classes Abstraites vs Interfaces

| | Abstract | Interface |
|--|:--------:|:---------:|
| Attributs | ✓ | ✗ (constantes) |
| Constructeur | ✓ | ✗ |
| Méthodes concrètes | ✓ | ✓ (default) |
| Héritage multiple | ✗ | ✓ |

## 6. Exceptions

```java
try {
    // code risqué
} catch (Exception e) {
    // traitement erreur
} finally {
    // toujours exécuté
}

throw new Exception();      // LANCER
void f() throws Exception   // DÉCLARER
```

## 7. Génériques

```java
class Bag<E> { }              // Déclaration
class Bag<E extends Item> { } // Contrainte
Bag<Potion> b = new Bag<>();  // Diamant (inférence)
```

## 8. Collections

| Besoin | Structure |
|--------|-----------|
| Liste indexée | `ArrayList<E>` |
| Insertions fréquentes | `LinkedList<E>` |
| Sans doublons | `HashSet<E>` |
| Trié sans doublons | `TreeSet<E>` |
| Clé → Valeur | `HashMap<K,V>` |

```java
for (E e : collection) { }  // For-each
iterator.hasNext() / .next() / .remove()
```

## 9. Lambda

```java
(params) -> expression
(x, y) -> x + y
liste.forEach(e -> System.out.println(e));
liste.sort((a,b) -> a.compareTo(b));
```

## 10. SOLID

| | Principe | Terme anglais |
|--|----------|---------------|
| **S** | 1 classe = 1 responsabilité | Single Responsibility |
| **O** | Ouvert extension, fermé modif | Open/Closed |
| **L** | Fille substituable à Mère | Liskov Substitution |
| **I** | Interfaces spécialisées | Interface Segregation |
| **D** | Dépendre des abstractions | Dependency Inversion |

## 11. Design Patterns

**Singleton:** 1 seule instance
```java
private static Instance inst;
private Singleton() {}
public static getInstance() { if(inst==null) inst=new...; return inst; }
```

**Factory:** Déléguer création aux sous-classes

---

# 🅱️ VERSO — UML

## 1. Diagrammes de Classe — RELATIONS

| Relation | Trait | Extrémité | Mermaid |
|----------|:-----:|:---------:|:-------:|
| Association | ─── | aucune | `--` |
| Dépendance | - - - | ▶ pleine | `..>` |
| **Héritage** | ─── | ▷ **vide** | `--|>` |
| **Implémentation** | - - - | ▷ **vide** | `..|>` |
| **Agrégation** | ─── | ◇ **vide** | `o--` |
| **Composition** | ─── | ◆ **plein** | `*--` |

### Mnémotechnique
```
Héritage      = trait PLEIN  + triangle VIDE
Implémentation = trait POINTILLÉ + triangle VIDE
Agrégation ◇  = VIDE  → partie SURVIT
Composition ◆ = PLEIN → partie MEURT avec le tout
```

## 2. Visibilité UML

| Symbole | Visibilité |
|:-------:|------------|
| `+` | public |
| `#` | protected |
| `-` | private |
| `~` | package |

## 3. Multiplicités

| Notation | Signification |
|----------|---------------|
| `1` | Exactement 1 |
| `0..1` | 0 ou 1 |
| `*` | 0 ou plusieurs |
| `1..*` | Au moins 1 |

**Placement:** Multiplicité côté **CIBLE** (à l'opposé)
```
Entreprise "1" ────── "*" Employe
     ↑                      ↑
  côté E.              côté Emp.

"Combien de B pour 1 A?" → réponse côté B
```

## 4. Diagramme de Séquence

```
─────▶  Message synchrone (appel bloquant)
- - -▶  Message retour (réponse)
────>   Message asynchrone

Fragments: alt (if/else), opt (if), loop, par
```

## 5. Cas d'Utilisation

```
<<include>>  → inclusion OBLIGATOIRE (A inclut toujours B)
<<extend>>   → extension OPTIONNELLE (B peut étendre A)
───▷         → généralisation (A est un type de B)
```

**Direction des flèches:**
- Include: A ----<<include>>----> B (A vers B)
- Extend: B ----<<extend>>----> A (B vers A, B étend A)

## 6. États-Transitions

```
● État initial (cercle plein)
◉ État final (cercle dans cercle)
⬜ État (rectangle arrondi)

Transition: événement [garde] / action
```

**Actions internes:**
- `entry/` → à l'entrée
- `exit/` → à la sortie  
- `do/` → pendant l'état

## 7. Cycle en V

```
Spécification (MOA) ←──────→ Recette
Conception Générale ←──────→ Tests Intégration
Conception Détaillée ←─────→ Tests Unitaires
         Codage
```

---

## 💡 PIÈGES FRÉQUENTS

1. **Aliasing**: `tab2 = tab1` → MÊME référence !
2. **==** compare les adresses, **equals()** le contenu
3. **Constructeur**: même nom que classe, PAS de return type
4. **super()** doit être en 1ère ligne du constructeur
5. **Tableaux génériques**: `new T[10]` INTERDIT
6. **Composition ◆** = losange PLEIN (partie meurt)
7. **Agrégation ◇** = losange VIDE (partie survit)
8. **Include** = obligatoire, **Extend** = optionnel
