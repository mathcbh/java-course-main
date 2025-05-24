# Fiche de Révision Complète - Java L3 Objet

## Table des Matières

1. [Outils et Environnement](#1-outils-et-environnement)
2. [Syntaxe Java de Base](#2-syntaxe-java-de-base)
3. [Exécution de Programmes Java](#3-exécution-de-programmes-java)
4. [Contrats et Fail-Fast](#4-contrats-et-fail-fast)
5. [Objets et Interfaces](#5-objets-et-interfaces)
6. [Héritage et Polymorphisme](#6-héritage-et-polymorphisme)
7. [Gestion d'Exceptions](#7-gestion-dexceptions)
8. [Généricité](#8-généricité)
9. [Collections](#9-collections)
10. [Tests Unitaires avec JUnit](#10-tests-unitaires-avec-junit)
11. [Maven](#11-maven)
12. [Concepts Avancés](#12-concepts-avancés)

---

## 1. Outils et Environnement

### 1.1 Git - Gestion de Version

#### Concepts Fondamentaux
- **Repository** : Dépôt contenant l'historique complet du projet
- **Commit** : Instantané de votre code à un moment donné
- **Branch** : Ligne de développement parallèle
- **Merge** : Fusion de deux branches
- **Remote** : Dépôt distant (ex: GitHub)

#### Commandes Essentielles
```bash
# Configuration initiale
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"

# Commandes de base
git init                    # Initialiser un dépôt
git clone <url>            # Cloner un dépôt distant
git status                 # Voir l'état des fichiers
git add <fichier>          # Ajouter un fichier à l'index
git commit -m "message"    # Faire un commit
git push                   # Pousser vers le remote
git pull                   # Récupérer depuis le remote

# Branches
git branch <nom>           # Créer une branche
git checkout <branche>     # Changer de branche
git merge <branche>        # Fusionner une branche
```

#### Bonnes Pratiques
- Commits fréquents avec messages descriptifs
- Une fonctionnalité = une branche
- Toujours tester avant de merger
- Utiliser `.gitignore` pour exclure les fichiers temporaires

### 1.2 Shell et Ligne de Commande

#### Navigation
```bash
pwd                        # Répertoire courant
ls / ls -la               # Lister les fichiers
cd <répertoire>           # Changer de répertoire
mkdir <nom>               # Créer un répertoire
```

#### Chemins
- **Absolu** : `/home/user/projet` (depuis la racine)
- **Relatif** : `../autre_dossier` (depuis position actuelle)

### 1.3 VS Code et Outils de Développement

#### Configuration
- Installation du JDK
- Extension Java pour VS Code
- Configuration du classpath
- Intégration Git

---

## 2. Syntaxe Java de Base

### 2.1 Variables et Types Primitifs

```java
// Types primitifs
int age = 25;
double prix = 19.99;
boolean estVrai = true;
char lettre = 'A';
String nom = "Jean";  // Classe, pas primitif

// Déclaration et initialisation
int x;                // Déclaration
x = 10;              // Initialisation
int y = 5;           // Déclaration + initialisation
```

### 2.2 Méthodes

```java
// Structure d'une méthode
public static int additionner(int a, int b) {
    return a + b;
}

// Méthode sans retour
public static void afficher(String message) {
    System.out.println(message);
}

// Méthode principale
public static void main(String[] args) {
    // Point d'entrée du programme
}
```

### 2.3 Classes et Objets

#### Définition d'une classe
```java
public class Personne {
    // Variables d'instance (privées)
    private String nom;
    private int age;
    
    // Constructeur
    public Personne(String nom, int age) {
        this.nom = nom;
        this.age = age;
    }
    
    // Méthodes d'accès (getters)
    public String getNom() {
        return nom;
    }
    
    public int getAge() {
        return age;
    }
    
    // Méthodes de modification (setters)
    public void setAge(int age) {
        this.age = age;
    }
    
    // Méthode d'instance
    public void sePresenter() {
        System.out.println("Je suis " + nom + ", j'ai " + age + " ans.");
    }
}
```

#### Utilisation
```java
public class TestPersonne {
    public static void main(String[] args) {
        Personne p = new Personne("Alice", 30);
        p.sePresenter();
        p.setAge(31);
        System.out.println("Nouvel âge : " + p.getAge());
    }
}
```

### 2.4 Variables et Méthodes Statiques

```java
public class Compteur {
    private static int count = 0;  // Variable de classe
    private int id;                // Variable d'instance
    
    public Compteur() {
        count++;
        this.id = count;
    }
    
    public static int getCount() {  // Méthode statique
        return count;
    }
    
    public int getId() {           // Méthode d'instance
        return id;
    }
}
```

### 2.5 Packages

```java
// Déclaration du package (première ligne du fichier)
package com.exemple.monprojet;

// Imports
import java.util.List;
import java.util.ArrayList;

public class MaClasse {
    // ...
}
```

#### Structure des Dossiers
```
src/
  com/
    exemple/
      monprojet/
        MaClasse.java
```

### 2.6 Varargs

```java
// Méthode avec nombre variable d'arguments
public static int somme(int... nombres) {
    int total = 0;
    for (int nombre : nombres) {
        total += nombre;
    }
    return total;
}

// Utilisation
int resultat1 = somme(1, 2, 3);
int resultat2 = somme(1, 2, 3, 4, 5);
```

---

## 3. Exécution de Programmes Java

### 3.1 Compilation et Exécution Simple

```bash
# Compilation
javac MonProgramme.java

# Exécution
java MonProgramme

# Avec arguments
java MonProgramme arg1 arg2
```

### 3.2 Avec Packages

```bash
# Structure des fichiers
src/
  com/
    exemple/
      MonProgramme.java

# Compilation vers dossier target
javac -d target src/com/exemple/MonProgramme.java

# Exécution
java -cp target com.exemple.MonProgramme
```

### 3.3 Classpath

```bash
# Spécifier le classpath
java -cp "chemin1:chemin2" com.exemple.MonProgramme

# Sous Windows (séparateur ;)
java -cp "chemin1;chemin2" com.exemple.MonProgramme
```

---

## 4. Contrats et Fail-Fast

### 4.1 Principe du Contrat

Un **contrat** définit ce qu'une méthode s'engage à faire :
- **Préconditions** : Ce qui doit être vrai à l'entrée
- **Postconditions** : Ce qui sera vrai à la sortie
- **Effets de bord** : Modifications apportées

#### Exemple avec Javadoc
```java
/**
 * Calcule la racine carrée d'un nombre.
 * 
 * @param x le nombre dont on veut la racine (doit être >= 0)
 * @return la racine carrée de x
 * @throws IllegalArgumentException si x < 0
 */
public static double racineCarree(double x) {
    if (x < 0) {
        throw new IllegalArgumentException("x doit être positif");
    }
    return Math.sqrt(x);
}
```

### 4.2 Fail-Fast

**Principe** : Détecter et signaler les erreurs le plus tôt possible.

```java
public class BankAccount {
    private double balance;
    
    public BankAccount(double initialBalance) {
        if (initialBalance < 0) {
            throw new IllegalArgumentException("Le solde initial ne peut être négatif");
        }
        this.balance = initialBalance;
    }
    
    public void withdraw(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Le montant doit être positif");
        }
        if (amount > balance) {
            throw new IllegalStateException("Solde insuffisant");
        }
        balance -= amount;
    }
}
```

### 4.3 Javadoc

#### Tags Importants
```java
/**
 * Description courte de la méthode.
 * Description plus détaillée si nécessaire.
 * 
 * @param nomParam description du paramètre
 * @return description de la valeur retournée
 * @throws ExceptionType description de quand l'exception est levée
 * @since version
 * @author nom de l'auteur
 */
```

#### Génération
```bash
javadoc -d docs src/*.java
```

---

## 5. Objets et Interfaces

### 5.1 Variables et Méthodes d'Instance

```java
public class Voiture {
    // Variables d'instance
    private String marque;
    private String modele;
    private int vitesse;
    
    // Constructeur
    public Voiture(String marque, String modele) {
        this.marque = marque;
        this.modele = modele;
        this.vitesse = 0;
    }
    
    // Méthode d'instance
    public void accelerer(int increment) {
        if (increment < 0) {
            throw new IllegalArgumentException("L'incrément doit être positif");
        }
        this.vitesse += increment;
    }
    
    public void freiner(int decrement) {
        if (decrement < 0) {
            throw new IllegalArgumentException("Le décrément doit être positif");
        }
        this.vitesse = Math.max(0, this.vitesse - decrement);
    }
    
    // Getters
    public String getMarque() { return marque; }
    public String getModele() { return modele; }
    public int getVitesse() { return vitesse; }
}
```

### 5.2 Interfaces

#### Définition d'une Interface
```java
public interface Movable {
    // Méthodes abstraites (implicitement public abstract)
    void move(double distance);
    double getPosition();
    boolean canAccelerate();
    
    // Méthode par défaut (Java 8+)
    default void stop() {
        // Implémentation par défaut
        System.out.println("Arrêt de l'objet");
    }
    
    // Méthode statique
    static double convertKmToMiles(double km) {
        return km * 0.621371;
    }
}
```

#### Implémentation d'une Interface
```java
public class Car implements Movable {
    private double position = 0;
    private double speed = 0;
    
    @Override
    public void move(double distance) {
        if (distance < 0) {
            throw new IllegalArgumentException("Distance ne peut être négative");
        }
        position += distance;
    }
    
    @Override
    public double getPosition() {
        return position;
    }
    
    @Override
    public boolean canAccelerate() {
        return speed < 200; // Vitesse max
    }
}
```

### 5.3 Méthodes de la Classe Object

#### toString()
```java
public class Personne {
    private String nom;
    private int age;
    
    // Constructeur...
    
    @Override
    public String toString() {
        return "Personne{nom='" + nom + "', age=" + age + "}";
    }
}
```

#### equals() et hashCode()
```java
public class Personne {
    private String nom;
    private int age;
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        
        Personne personne = (Personne) obj;
        return age == personne.age && 
               Objects.equals(nom, personne.nom);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(nom, age);
    }
}
```

### 5.4 Exercice : Three Dice

```java
public class TripletOfDice {
    private final int sides;
    private int die1, die2, die3;
    private Random random;
    
    /**
     * Crée un triplet de dés.
     * @param sides nombre de faces de chaque dé (doit être > 0)
     */
    public TripletOfDice(int sides) {
        if (sides <= 0) {
            throw new IllegalArgumentException("Le nombre de faces doit être positif");
        }
        this.sides = sides;
        this.random = new Random();
        rollAllDice();
    }
    
    /**
     * Lance tous les dés.
     */
    public void rollAllDice() {
        die1 = random.nextInt(sides) + 1;
        die2 = random.nextInt(sides) + 1;
        die3 = random.nextInt(sides) + 1;
    }
    
    /**
     * Lance un dé spécifique.
     * @param dieNumber numéro du dé (1, 2 ou 3)
     */
    public void rollOneDie(int dieNumber) {
        if (dieNumber < 1 || dieNumber > 3) {
            throw new IllegalArgumentException("Le numéro de dé doit être 1, 2 ou 3");
        }
        
        int newValue = random.nextInt(sides) + 1;
        switch (dieNumber) {
            case 1: die1 = newValue; break;
            case 2: die2 = newValue; break;
            case 3: die3 = newValue; break;
        }
    }
    
    public int getFirstDie() { return die1; }
    public int getSecondDie() { return die2; }
    public int getThirdDie() { return die3; }
    
    public int getTotal() {
        return die1 + die2 + die3;
    }
}
```

---

## 6. Héritage et Polymorphisme

### 6.1 Héritage de Classes

```java
// Classe parent
public class Animal {
    protected String nom;
    protected int age;
    
    public Animal(String nom, int age) {
        this.nom = nom;
        this.age = age;
    }
    
    public void dormir() {
        System.out.println(nom + " dort");
    }
    
    public void manger() {
        System.out.println(nom + " mange");
    }
    
    // Méthode à redéfinir
    public void faireDuBruit() {
        System.out.println(nom + " fait du bruit");
    }
}

// Classe enfant
public class Chien extends Animal {
    private String race;
    
    public Chien(String nom, int age, String race) {
        super(nom, age);  // Appel au constructeur parent
        this.race = race;
    }
    
    @Override
    public void faireDuBruit() {
        System.out.println(nom + " aboie : Woof!");
    }
    
    // Méthode spécifique au chien
    public void donnerLaPatte() {
        System.out.println(nom + " donne la patte");
    }
}
```

### 6.2 Polymorphisme

```java
public class TestPolymorphisme {
    public static void main(String[] args) {
        // Polymorphisme : référence de type parent, objet de type enfant
        Animal[] animaux = {
            new Chien("Rex", 5, "Berger"),
            new Chat("Minou", 3, "Persan"),
            new Animal("Inconnu", 1)
        };
        
        // La bonne méthode est appelée selon le type réel de l'objet
        for (Animal animal : animaux) {
            animal.faireDuBruit();  // Polymorphisme en action
        }
    }
}
```

### 6.3 Classes Abstraites

```java
public abstract class Forme {
    protected String couleur;
    
    public Forme(String couleur) {
        this.couleur = couleur;
    }
    
    // Méthode abstraite - doit être implémentée par les sous-classes
    public abstract double calculerAire();
    
    // Méthode concrète
    public void afficherCouleur() {
        System.out.println("Couleur : " + couleur);
    }
}

public class Cercle extends Forme {
    private double rayon;
    
    public Cercle(String couleur, double rayon) {
        super(couleur);
        this.rayon = rayon;
    }
    
    @Override
    public double calculerAire() {
        return Math.PI * rayon * rayon;
    }
}
```

### 6.4 Héritage d'Interfaces

```java
// Interface parent
public interface Vehicule {
    void demarrer();
    void arreter();
}

// Interface enfant
public interface VehiculeElectrique extends Vehicule {
    void charger();
    int getNiveauBatterie();
}

// Implémentation
public class TeslaModelS implements VehiculeElectrique {
    private int niveauBatterie = 100;
    private boolean enMarche = false;
    
    @Override
    public void demarrer() {
        if (niveauBatterie > 0) {
            enMarche = true;
            System.out.println("Tesla démarrée silencieusement");
        }
    }
    
    @Override
    public void arreter() {
        enMarche = false;
        System.out.println("Tesla arrêtée");
    }
    
    @Override
    public void charger() {
        niveauBatterie = 100;
        System.out.println("Batterie rechargée");
    }
    
    @Override
    public int getNiveauBatterie() {
        return niveauBatterie;
    }
}
```

---

## 7. Gestion d'Exceptions

### 7.1 Hiérarchie des Exceptions

```
Throwable
├── Error (erreurs système, non récupérables)
└── Exception
    ├── RuntimeException (exceptions non vérifiées)
    │   ├── IllegalArgumentException
    │   ├── IllegalStateException
    │   ├── NullPointerException
    │   └── ...
    └── Exceptions vérifiées
        ├── IOException
        ├── FileNotFoundException
        └── ...
```

### 7.2 Exceptions Vérifiées vs Non Vérifiées

#### Exceptions Non Vérifiées (RuntimeException)
```java
public class CalculateurAge {
    public static int calculerAge(int anneeNaissance) {
        int anneeActuelle = 2024;
        if (anneeNaissance > anneeActuelle) {
            // Pas besoin de déclarer dans la signature
            throw new IllegalArgumentException("Année de naissance invalide");
        }
        return anneeActuelle - anneeNaissance;
    }
}
```

#### Exceptions Vérifiées
```java
import java.io.*;

public class LecteurFichier {
    // DOIT déclarer l'exception dans la signature
    public static String lireFichier(String nomFichier) throws IOException {
        BufferedReader reader = new BufferedReader(new FileReader(nomFichier));
        try {
            return reader.readLine();
        } finally {
            reader.close();
        }
    }
    
    public static void utiliserLecteur() {
        try {
            String contenu = lireFichier("test.txt");
            System.out.println(contenu);
        } catch (IOException e) {
            System.err.println("Erreur de lecture : " + e.getMessage());
        }
    }
}
```

### 7.3 Try-Catch-Finally

```java
public class GestionExceptions {
    public static void exempleComplet() {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream("fichier.txt");
            // Code qui peut lever une exception
            int data = fis.read();
            System.out.println("Premier byte : " + data);
        } catch (FileNotFoundException e) {
            System.err.println("Fichier non trouvé : " + e.getMessage());
        } catch (IOException e) {
            System.err.println("Erreur de lecture : " + e.getMessage());
        } finally {
            // TOUJOURS exécuté
            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    System.err.println("Erreur lors de la fermeture");
                }
            }
        }
    }
}
```

### 7.4 Try-with-Resources (Java 7+)

```java
public class GestionRessources {
    public static void exempleAvecResources() {
        // Les ressources sont automatiquement fermées
        try (FileInputStream fis = new FileInputStream("fichier.txt");
             BufferedReader reader = new BufferedReader(new InputStreamReader(fis))) {
            
            String ligne = reader.readLine();
            System.out.println(ligne);
            
        } catch (IOException e) {
            System.err.println("Erreur : " + e.getMessage());
        }
        // fis et reader sont automatiquement fermés ici
    }
}
```

### 7.5 Créer ses Propres Exceptions

```java
// Exception vérifiée personnalisée
public class SoldeInsuffisantException extends Exception {
    private double soldeActuel;
    private double montantDemande;
    
    public SoldeInsuffisantException(double soldeActuel, double montantDemande) {
        super("Solde insuffisant : " + soldeActuel + " < " + montantDemande);
        this.soldeActuel = soldeActuel;
        this.montantDemande = montantDemande;
    }
    
    public double getSoldeActuel() { return soldeActuel; }
    public double getMontantDemande() { return montantDemande; }
}

// Utilisation
public class CompteBancaire {
    private double solde;
    
    public void retirer(double montant) throws SoldeInsuffisantException {
        if (montant > solde) {
            throw new SoldeInsuffisantException(solde, montant);
        }
        solde -= montant;
    }
}
```

---

## 8. Généricité

### 8.1 Classes Génériques

```java
// Classe générique simple
public class Pair<T, U> {
    private T first;
    private U second;
    
    public Pair(T first, U second) {
        this.first = first;
        this.second = second;
    }
    
    public T getFirst() { return first; }
    public U getSecond() { return second; }
    
    public void setFirst(T first) { this.first = first; }
    public void setSecond(U second) { this.second = second; }
    
    @Override
    public String toString() {
        return "(" + first + ", " + second + ")";
    }
}

// Utilisation
public class TestPair {
    public static void main(String[] args) {
        Pair<String, Integer> nameAge = new Pair<>("Alice", 25);
        Pair<Double, Double> coordinates = new Pair<>(3.14, 2.71);
        
        System.out.println(nameAge);      // (Alice, 25)
        System.out.println(coordinates);  // (3.14, 2.71)
    }
}
```

### 8.2 Méthodes Génériques

```java
public class GenericUtils {
    // Méthode générique statique
    public static <T> void swap(T[] array, int i, int j) {
        T temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
    
    // Méthode générique avec contraintes
    public static <T extends Comparable<T>> T max(T a, T b) {
        return a.compareTo(b) > 0 ? a : b;
    }
    
    // Méthode générique d'instance
    public <E> void printArray(E[] array) {
        for (E element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }
}
```

### 8.3 Interfaces Génériques

```java
public interface Container<T> {
    void add(T item);
    T get(int index);
    int size();
    boolean isEmpty();
}

public class SimpleContainer<T> implements Container<T> {
    private List<T> items = new ArrayList<>();
    
    @Override
    public void add(T item) {
        items.add(item);
    }
    
    @Override
    public T get(int index) {
        return items.get(index);
    }
    
    @Override
    public int size() {
        return items.size();
    }
    
    @Override
    public boolean isEmpty() {
        return items.isEmpty();
    }
}
```

### 8.4 Wildcards

```java
import java.util.*;

public class WildcardExample {
    // Wildcard avec borne supérieure (? extends)
    public static double sumOfNumbers(List<? extends Number> numbers) {
        double sum = 0.0;
        for (Number num : numbers) {
            sum += num.doubleValue();
        }
        return sum;
    }
    
    // Wildcard avec borne inférieure (? super)
    public static void addNumbers(List<? super Integer> list) {
        list.add(1);
        list.add(2);
        list.add(3);
    }
    
    // Wildcard non borné (?)
    public static void printList(List<?> list) {
        for (Object item : list) {
            System.out.println(item);
        }
    }
}
```

---

## 9. Collections

### 9.1 Hiérarchie des Collections

```
Iterable<T>
└── Collection<T>
    ├── List<T>
    │   ├── ArrayList<T>
    │   ├── LinkedList<T>
    │   └── Vector<T>
    ├── Set<T>
    │   ├── HashSet<T>
    │   ├── LinkedHashSet<T>
    │   └── TreeSet<T>
    └── Queue<T>
        ├── LinkedList<T>
        └── PriorityQueue<T>

Map<K,V> (interface séparée)
├── HashMap<K,V>
├── LinkedHashMap<K,V>
└── TreeMap<K,V>
```

### 9.2 Listes (List)

```java
import java.util.*;

public class ExemplesListes {
    public static void main(String[] args) {
        // ArrayList - accès rapide par index
        List<String> arrayList = new ArrayList<>();
        arrayList.add("Premier");
        arrayList.add("Deuxième");
        arrayList.add(1, "Inséré");  // Insertion à l'index 1
        
        // LinkedList - insertion/suppression rapide
        List<Integer> linkedList = new LinkedList<>();
        linkedList.add(10);
        linkedList.add(20);
        linkedList.add(0, 5);  // Insertion au début
        
        // Parcours avec enhanced for
        for (String item : arrayList) {
            System.out.println(item);
        }
        
        // Accès par index
        String premier = arrayList.get(0);
        arrayList.set(0, "Modifié");  // Modification
        
        // Recherche
        boolean contient = arrayList.contains("Deuxième");
        int index = arrayList.indexOf("Deuxième");
    }
}
```

### 9.3 Ensembles (Set)

```java
import java.util.*;

public class ExemplesEnsembles {
    public static void main(String[] args) {
        // HashSet - pas d'ordre, accès O(1)
        Set<String> hashSet = new HashSet<>();
        hashSet.add("Rouge");
        hashSet.add("Vert");
        hashSet.add("Bleu");
        hashSet.add("Rouge");  // Pas de doublons
        
        // LinkedHashSet - ordre d'insertion préservé
        Set<String> linkedHashSet = new LinkedHashSet<>();
        linkedHashSet.add("Alpha");
        linkedHashSet.add("Beta");
        linkedHashSet.add("Gamma");
        
        // TreeSet - ordre naturel (sorted)
        Set<Integer> treeSet = new TreeSet<>();
        treeSet.add(50);
        treeSet.add(10);
        treeSet.add(30);
        // Affiché dans l'ordre : 10, 30, 50
        
        // Opérations sur les ensembles
        Set<String> set1 = new HashSet<>(Arrays.asList("A", "B", "C"));
        Set<String> set2 = new HashSet<>(Arrays.asList("B", "C", "D"));
        
        // Union
        Set<String> union = new HashSet<>(set1);
        union.addAll(set2);  // {A, B, C, D}
        
        // Intersection
        Set<String> intersection = new HashSet<>(set1);
        intersection.retainAll(set2);  // {B, C}
        
        // Différence
        Set<String> difference = new HashSet<>(set1);
        difference.removeAll(set2);  // {A}
    }
}
```

### 9.4 Maps

```java
import java.util.*;

public class ExemplesMaps {
    public static void main(String[] args) {
        // HashMap - pas d'ordre
        Map<String, Integer> hashMap = new HashMap<>();
        hashMap.put("Alice", 25);
        hashMap.put("Bob", 30);
        hashMap.put("Charlie", 35);
        
        // LinkedHashMap - ordre d'insertion
        Map<String, String> linkedHashMap = new LinkedHashMap<>();
        linkedHashMap.put("pays", "France");
        linkedHashMap.put("capitale", "Paris");
        linkedHashMap.put("population", "67M");
        
        // TreeMap - ordre des clés
        Map<Integer, String> treeMap = new TreeMap<>();
        treeMap.put(3, "Trois");
        treeMap.put(1, "Un");
        treeMap.put(2, "Deux");
        // Ordre : 1->Un, 2->Deux, 3->Trois
        
        // Opérations sur les maps
        Integer age = hashMap.get("Alice");  // 25
        boolean exists = hashMap.containsKey("Bob");
        Integer defaultAge = hashMap.getOrDefault("David", 0);
        
        // Parcours
        for (Map.Entry<String, Integer> entry : hashMap.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }
        
        // Parcours des clés seulement
        for (String nom : hashMap.keySet()) {
            System.out.println(nom);
        }
        
        // Parcours des valeurs seulement
        for (Integer age : hashMap.values()) {
            System.out.println(age);
        }
    }
}
```

### 9.5 Iterators

```java
import java.util.*;

public class ExemplesIterators {
    public static void main(String[] args) {
        List<String> liste = new ArrayList<>(Arrays.asList("A", "B", "C", "D"));
        
        // Iterator basique
        Iterator<String> iter = liste.iterator();
        while (iter.hasNext()) {
            String element = iter.next();
            if (element.equals("B")) {
                iter.remove();  // Suppression sécurisée
            }
        }
        
        // ListIterator (bidirectionnel)
        ListIterator<String> listIter = liste.listIterator();
        while (listIter.hasNext()) {
            String element = listIter.next();
            if (element.equals("C")) {
                listIter.set("C_modifié");  // Modification
                listIter.add("C_ajouté");   // Ajout
            }
        }
        
        // Parcours en arrière
        while (listIter.hasPrevious()) {
            System.out.println(listIter.previous());
        }
    }
}
```

### 9.6 Implémentation d'Iterable

```java
public class PairIterator<E> implements Iterator<E> {
    private Pair<E, E> pair;
    private int position = 0;
    
    public PairIterator(Pair<E, E> pair) {
        this.pair = pair;
    }
    
    @Override
    public boolean hasNext() {
        return position < 2;
    }
    
    @Override
    public E next() {
        if (!hasNext()) {
            throw new NoSuchElementException();
        }
        
        E result = (position == 0) ? pair.getFirst() : pair.getSecond();
        position++;
        return result;
    }
}

public class HomogeneousPair<E> implements Iterable<E> {
    private E first;
    private E second;
    
    public HomogeneousPair(E first, E second) {
        this.first = first;
        this.second = second;
    }
    
    @Override
    public Iterator<E> iterator() {
        return new PairIterator<>(new Pair<>(first, second));
    }
    
    // Utilisation avec enhanced for loop
    public static void main(String[] args) {
        HomogeneousPair<String> pair = new HomogeneousPair<>("Hello", "World");
        
        for (String word : pair) {  // Utilise notre iterator
            System.out.println(word);
        }
    }
}
```

### 9.7 Exercice : Collections

```java
import java.util.*;

public class GestionEtudiants {
    public static void main(String[] args) {
        // Exercice : gestion d'étudiants avec notes
        Scanner scanner = new Scanner(System.in);
        
        // Map pour associer nom -> liste de notes
        Map<String, List<Double>> etudiants = new HashMap<>();
        
        // Saisie des données
        System.out.println("Entrez les données (nom note, 'fin' pour terminer) :");
        String input;
        while (!(input = scanner.nextLine()).equals("fin")) {
            String[] parts = input.split(" ");
            if (parts.length == 2) {
                String nom = parts[0];
                double note = Double.parseDouble(parts[1]);
                
                etudiants.computeIfAbsent(nom, k -> new ArrayList<>()).add(note);
            }
        }
        
        // Calcul des moyennes
        Map<String, Double> moyennes = new HashMap<>();
        for (Map.Entry<String, List<Double>> entry : etudiants.entrySet()) {
            List<Double> notes = entry.getValue();
            double moyenne = notes.stream().mapToDouble(Double::doubleValue).average().orElse(0.0);
            moyennes.put(entry.getKey(), moyenne);
        }
        
        // Affichage trié par moyenne décroissante
        moyennes.entrySet().stream()
                .sorted(Map.Entry.<String, Double>comparingByValue().reversed())
                .forEach(entry -> System.out.println(entry.getKey() + " : " + entry.getValue()));
    }
}
```

---

## 10. Tests Unitaires avec JUnit

### 10.1 Configuration JUnit Jupiter

#### Dans un projet Maven
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.9.2</version>
    <scope>test</scope>
</dependency>
```

### 10.2 Tests de Base

```java
// Classe à tester
public class Calculatrice {
    public int additionner(int a, int b) {
        return a + b;
    }
    
    public double diviser(double a, double b) {
        if (b == 0) {
            throw new IllegalArgumentException("Division par zéro");
        }
        return a / b;
    }
    
    public boolean estPair(int nombre) {
        return nombre % 2 == 0;
    }
}
```

```java
// Classe de test
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class CalculatriceTest {
    private Calculatrice calc;
    
    @BeforeEach
    void setUp() {
        calc = new Calculatrice();
    }
    
    @Test
    void testAdditionner() {
        // Given (Arrange)
        int a = 5;
        int b = 3;
        
        // When (Act)
        int resultat = calc.additionner(a, b);
        
        // Then (Assert)
        assertEquals(8, resultat, "5 + 3 devrait égaler 8");
    }
    
    @Test
    void testDiviserNormale() {
        double resultat = calc.diviser(10, 2);
        assertEquals(5.0, resultat, 0.001);  // Delta pour les doubles
    }
    
    @Test
    void testDiviserParZero() {
        // Test qu'une exception est bien levée
        IllegalArgumentException exception = assertThrows(
            IllegalArgumentException.class,
            () -> calc.diviser(10, 0),
            "Diviser par zéro devrait lever une exception"
        );
        
        assertEquals("Division par zéro", exception.getMessage());
    }
    
    @Test
    void testEstPair() {
        assertTrue(calc.estPair(4), "4 devrait être pair");
        assertFalse(calc.estPair(5), "5 ne devrait pas être pair");
        assertTrue(calc.estPair(0), "0 devrait être pair");
        assertFalse(calc.estPair(-3), "-3 ne devrait pas être pair");
    }
    
    @Test
    @DisplayName("Test avec valeurs négatives")
    void testAdditionnerNegatifs() {
        assertEquals(-2, calc.additionner(-5, 3));
        assertEquals(-8, calc.additionner(-5, -3));
    }
}
```

### 10.3 Annotations JUnit

```java
class TestAvecAnnotations {
    static DatabaseConnection db;
    
    @BeforeAll
    static void setUpDatabase() {
        // Exécuté une fois avant tous les tests
        db = new DatabaseConnection();
        System.out.println("Base de données initialisée");
    }
    
    @AfterAll
    static void tearDownDatabase() {
        // Exécuté une fois après tous les tests
        if (db != null) {
            db.close();
        }
        System.out.println("Base de données fermée");
    }
    
    @BeforeEach
    void setUp() {
        // Exécuté avant chaque test
        System.out.println("Préparation du test");
    }
    
    @AfterEach
    void tearDown() {
        // Exécuté après chaque test
        System.out.println("Nettoyage après test");
    }
    
    @Test
    @DisplayName("Ce test vérifie quelque chose d'important")
    void testImportant() {
        assertTrue(true);
    }
    
    @Test
    @Disabled("Test temporairement désactivé")
    void testDesactive() {
        fail("Ce test ne devrait pas s'exécuter");
    }
    
    @RepeatedTest(5)
    void testRepete() {
        // Exécuté 5 fois
        assertTrue(Math.random() >= 0);
    }
}
```

### 10.4 Tests Paramétrés

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.*;

class TestsParametres {
    
    @ParameterizedTest
    @ValueSource(ints = {2, 4, 6, 8, 10})
    void testNombresPairs(int nombre) {
        assertTrue(nombre % 2 == 0);
    }
    
    @ParameterizedTest
    @ValueSource(strings = {"", "   ", "\t", "\n"})
    void testChainesVides(String chaine) {
        assertTrue(chaine.trim().isEmpty());
    }
    
    @ParameterizedTest
    @CsvSource({
        "1, 1, 2",
        "2, 3, 5",
        "5, 7, 12",
        "-1, 1, 0"
    })
    void testAddition(int a, int b, int expected) {
        Calculatrice calc = new Calculatrice();
        assertEquals(expected, calc.additionner(a, b));
    }
    
    @ParameterizedTest
    @EnumSource(Month.class)
    void testMois(Month mois) {
        assertNotNull(mois);
        assertTrue(mois.getValue() >= 1 && mois.getValue() <= 12);
    }
}
```

### 10.5 Assertions Avancées

```java
import static org.junit.jupiter.api.Assertions.*;
import java.time.Duration;
import java.util.Arrays;
import java.util.List;

class AssertionsAvancees {
    
    @Test
    void testAssertions() {
        // Assertions de base
        assertEquals(4, 2 + 2);
        assertNotEquals(5, 2 + 2);
        assertTrue(2 > 1);
        assertFalse(1 > 2);
        assertNull(null);
        assertNotNull(new Object());
        
        // Assertions sur les collections
        List<String> expected = Arrays.asList("A", "B", "C");
        List<String> actual = Arrays.asList("A", "B", "C");
        assertIterableEquals(expected, actual);
        
        // Assertions sur les tableaux
        int[] expectedArray = {1, 2, 3};
        int[] actualArray = {1, 2, 3};
        assertArrayEquals(expectedArray, actualArray);
        
        // Assertions groupées
        assertAll("Personne",
            () -> assertEquals("John", person.getFirstName()),
            () -> assertEquals("Doe", person.getLastName()),
            () -> assertEquals(30, person.getAge())
        );
        
        // Test de timeout
        assertTimeout(Duration.ofSeconds(2), () -> {
            Thread.sleep(1000);  // Simulation d'opération lente
            return "Résultat";
        });
        
        // Test d'exception avec message personnalisé
        Exception exception = assertThrows(RuntimeException.class, () -> {
            throw new RuntimeException("Erreur de test");
        });
        assertEquals("Erreur de test", exception.getMessage());
    }
}
```

### 10.6 Test d'une Classe Complexe

```java
// Classe à tester
public class BankAccount {
    private String accountNumber;
    private double balance;
    private List<String> transactions;
    
    public BankAccount(String accountNumber, double initialBalance) {
        if (initialBalance < 0) {
            throw new IllegalArgumentException("Solde initial négatif");
        }
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
        this.transactions = new ArrayList<>();
    }
    
    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Montant doit être positif");
        }
        balance += amount;
        transactions.add("Dépôt: +" + amount);
    }
    
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount <= 0) {
            throw new IllegalArgumentException("Montant doit être positif");
        }
        if (amount > balance) {
            throw new InsufficientFundsException("Solde insuffisant");
        }
        balance -= amount;
        transactions.add("Retrait: -" + amount);
    }
    
    public double getBalance() { return balance; }
    public List<String> getTransactions() { return new ArrayList<>(transactions); }
}
```

```java
// Tests complets
class BankAccountTest {
    private BankAccount account;
    
    @BeforeEach
    void setUp() {
        account = new BankAccount("123456", 1000.0);
    }
    
    @Test
    void testCreationCompte() {
        assertEquals(1000.0, account.getBalance());
        assertTrue(account.getTransactions().isEmpty());
    }
    
    @Test
    void testCreationAvecSoldeNegatif() {
        assertThrows(IllegalArgumentException.class, 
            () -> new BankAccount("123", -100));
    }
    
    @Test
    void testDepot() {
        account.deposit(500);
        
        assertEquals(1500.0, account.getBalance());
        assertEquals(1, account.getTransactions().size());
        assertTrue(account.getTransactions().get(0).contains("Dépôt: +500"));
    }
    
    @Test
    void testDepotMontantNegatif() {
        assertThrows(IllegalArgumentException.class, 
            () -> account.deposit(-100));
    }
    
    @Test
    void testRetraitNormal() throws InsufficientFundsException {
        account.withdraw(300);
        
        assertEquals(700.0, account.getBalance());
        assertEquals(1, account.getTransactions().size());
        assertTrue(account.getTransactions().get(0).contains("Retrait: -300"));
    }
    
    @Test
    void testRetraitSoldeInsuffisant() {
        InsufficientFundsException exception = assertThrows(
            InsufficientFundsException.class,
            () -> account.withdraw(1500)
        );
        
        assertEquals("Solde insuffisant", exception.getMessage());
        assertEquals(1000.0, account.getBalance());  // Solde inchangé
    }
    
    @Test
    void testPlusieursTransactions() throws InsufficientFundsException {
        account.deposit(200);
        account.withdraw(300);
        account.deposit(100);
        
        assertEquals(1000.0, account.getBalance());  // 1000 + 200 - 300 + 100
        assertEquals(3, account.getTransactions().size());
    }
    
    @Test
    void testImmutabiliteListeTransactions() {
        account.deposit(100);
        List<String> transactions = account.getTransactions();
        
        // Modifier la liste retournée ne doit pas affecter l'original
        transactions.clear();
        
        assertEquals(1, account.getTransactions().size());
    }
}
```

---

## 11. Maven

### 11.1 Structure d'un Projet Maven

```
mon-projet/
├── pom.xml
└── src/
    ├── main/
    │   ├── java/
    │   │   └── com/
    │   │       └── exemple/
    │   │           └── MaClasse.java
    │   └── resources/
    │       └── config.properties
    └── test/
        ├── java/
        │   └── com/
        │       └── exemple/
        │           └── MaClasseTest.java
        └── resources/
            └── test-config.properties
```

### 11.2 Fichier POM Basique

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <modelVersion>4.0.0</modelVersion>
    
    <!-- Coordonnées du projet -->
    <groupId>com.exemple</groupId>
    <artifactId>mon-projet</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    
    <!-- Métadonnées -->
    <name>Mon Projet Java</name>
    <description>Description de mon projet</description>
    
    <!-- Propriétés -->
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <junit.version>5.9.2</junit.version>
    </properties>
    
    <!-- Dépendances -->
    <dependencies>
        <!-- JUnit pour les tests -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        
        <!-- Exemple d'autre dépendance -->
        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>31.1-jre</version>
        </dependency>
    </dependencies>
    
    <!-- Configuration des plugins -->
    <build>
        <plugins>
            <!-- Plugin pour compiler -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>17</source>
                    <target>17</target>
                </configuration>
            </plugin>
            
            <!-- Plugin pour les tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0-M9</version>
            </plugin>
        </plugins>
    </build>
</project>
```

### 11.3 Commandes Maven Essentielles

```bash
# Nettoyer le projet
mvn clean

# Compiler le code source
mvn compile

# Compiler et exécuter les tests
mvn test

# Packager en JAR
mvn package

# Installer dans le dépôt local
mvn install

# Exécuter une classe avec main
mvn exec:java -Dexec.mainClass="com.exemple.MaClasse"

# Voir les dépendances
mvn dependency:tree

# Générer la documentation
mvn javadoc:javadoc

# Cycle complet
mvn clean compile test package
```

### 11.4 Archétypes Maven

```bash
# Créer un projet avec l'archétype recommandé du cours
mvn archetype:generate \
  -DarchetypeGroupId=io.github.oliviercailloux \
  -DarchetypeArtifactId=java-archetype \
  -DgroupId=com.exemple \
  -DartifactId=mon-projet \
  -Dversion=1.0.0

# Archétype standard Maven
mvn archetype:generate \
  -DarchetypeGroupId=org.apache.maven.archetypes \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DgroupId=com.exemple \
  -DartifactId=mon-projet
```

### 11.5 Scopes des Dépendances

```xml
<dependencies>
    <!-- Scope par défaut : compile -->
    <dependency>
        <groupId>com.google.guava</groupId>
        <artifactId>guava</artifactId>
        <version>31.1-jre</version>
        <!-- <scope>compile</scope> -->
    </dependency>
    
    <!-- Scope test : uniquement pour les tests -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.9.2</version>
        <scope>test</scope>
    </dependency>
    
    <!-- Scope provided : fourni par l'environnement d'exécution -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
        <scope>provided</scope>
    </dependency>
    
    <!-- Scope runtime : uniquement à l'exécution -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.33</version>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

---

## 12. Concepts Avancés

### 12.1 Types Primitifs et Autoboxing

#### Types Primitifs vs Classes Wrapper
```java
public class AutoboxingExample {
    public static void main(String[] args) {
        // Types primitifs
        int primitiveInt = 42;
        double primitiveDouble = 3.14;
        boolean primitiveBool = true;
        
        // Classes wrapper
        Integer wrapperInt = 42;           // Autoboxing
        Double wrapperDouble = 3.14;       // Autoboxing  
        Boolean wrapperBool = true;        // Autoboxing
        
        // Conversion explicite
        Integer explicit = Integer.valueOf(42);
        int backToPrimitive = wrapperInt.intValue();  // Unboxing
        
        // Autoboxing/Unboxing automatique
        List<Integer> numbers = new ArrayList<>();
        numbers.add(10);          // Autoboxing int -> Integer
        int first = numbers.get(0);  // Unboxing Integer -> int
        
        // Attention aux comparaisons
        Integer a = 127;
        Integer b = 127;
        System.out.println(a == b);        // true (cache)
        
        Integer c = 128;
        Integer d = 128;
        System.out.println(c == d);        // false (pas de cache)
        System.out.println(c.equals(d));   // true (comparaison correcte)
        
        // Problème avec null
        Integer nullable = null;
        // int value = nullable;  // NullPointerException à l'unboxing
    }
}
```

### 12.2 Optionals

#### Utilisation de Optional
```java
import java.util.Optional;

public class OptionalExample {
    
    public static Optional<String> findUserById(int id) {
        // Simulation de recherche
        if (id == 1) {
            return Optional.of("Alice");
        } else if (id == 2) {
            return Optional.of("Bob");
        } else {
            return Optional.empty();
        }
    }
    
    public static void main(String[] args) {
        // Création d'Optional
        Optional<String> empty = Optional.empty();
        Optional<String> notEmpty = Optional.of("Hello");
        Optional<String> nullable = Optional.ofNullable(getString()); // peut être null
        
        // Vérification de présence
        if (notEmpty.isPresent()) {
            System.out.println(notEmpty.get());
        }
        
        // Méthodes fonctionnelles
        notEmpty.ifPresent(System.out::println);  // Affiche si présent
        
        // Valeur par défaut
        String result = empty.orElse("Valeur par défaut");
        String result2 = empty.orElseGet(() -> computeDefault());
        
        // Exception si absent
        try {
            String value = empty.orElseThrow(() -> new RuntimeException("Valeur absente"));
        } catch (RuntimeException e) {
            System.out.println("Exception attrapée : " + e.getMessage());
        }
        
        // Transformation
        Optional<Integer> length = notEmpty.map(String::length);
        Optional<String> upper = notEmpty.map(String::toUpperCase);
        
        // Filtrage
        Optional<String> filtered = notEmpty.filter(s -> s.length() > 3);
        
        // Chaînage
        Optional<String> user = findUserById(1);
        String message = user
            .filter(name -> name.length() > 3)
            .map(name -> "Bonjour " + name)
            .orElse("Utilisateur non trouvé");
    }
    
    // Anti-pattern : ne pas faire ça
    public static String badExample(int id) {
        Optional<String> user = findUserById(id);
        if (user.isPresent()) {
            return user.get();
        } else {
            return null;  // Retour à null, perd l'intérêt d'Optional
        }
    }
    
    // Bonne pratique
    public static Optional<String> goodExample(int id) {
        return findUserById(id)
            .map(name -> name.toUpperCase());
    }
}
```

### 12.3 Protection contre les Références Null

#### Stratégies de Protection
```java
import java.util.Objects;
import java.util.Optional;

public class NullSafety {
    
    // 1. Validation des paramètres
    public static double divide(Double a, Double b) {
        Objects.requireNonNull(a, "Le dividende ne peut être null");
        Objects.requireNonNull(b, "Le diviseur ne peut être null");
        
        if (b == 0.0) {
            throw new IllegalArgumentException("Division par zéro");
        }
        return a / b;
    }
    
    // 2. Utilisation d'Optional pour les retours
    public static Optional<String> findConfigValue(String key) {
        // Au lieu de retourner null
        Properties config = loadConfig();
        String value = config.getProperty(key);
        return Optional.ofNullable(value);
    }
    
    // 3. Collections non nulles
    public static List<String> getItems() {
        // Retourner une liste vide plutôt que null
        List<String> items = loadItemsFromDatabase();
        return items != null ? items : Collections.emptyList();
    }
    
    // 4. Constructeur défensif
    public static class Person {
        private final String name;
        private final List<String> hobbies;
        
        public Person(String name, List<String> hobbies) {
            this.name = Objects.requireNonNull(name, "Le nom est obligatoire");
            // Copie défensive
            this.hobbies = hobbies != null 
                ? new ArrayList<>(hobbies) 
                : new ArrayList<>();
        }
        
        public String getName() {
            return name;  // Jamais null
        }
        
        public List<String> getHobbies() {
            return new ArrayList<>(hobbies);  // Copie défensive
        }
    }
    
    // 5. Pattern Null Object
    interface Logger {
        void log(String message);
    }
    
    static class ConsoleLogger implements Logger {
        public void log(String message) {
            System.out.println(message);
        }
    }
    
    static class NullLogger implements Logger {
        public void log(String message) {
            // Ne fait rien
        }
    }
    
    public static Logger getLogger(boolean enabled) {
        return enabled ? new ConsoleLogger() : new NullLogger();
    }
}
```

### 12.4 Comparators et Tri

```java
import java.util.*;
import java.util.stream.Collectors;

public class ComparatorExamples {
    
    static class Person {
        private String name;
        private int age;
        private String city;
        
        // Constructeur, getters...
        
        @Override
        public String toString() {
            return name + "(" + age + "," + city + ")";
        }
    }
    
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 30, "Paris"),
            new Person("Bob", 25, "Lyon"),
            new Person("Charlie", 35, "Paris"),
            new Person("David", 25, "Marseille")
        );
        
        // 1. Tri par âge (ordre naturel)
        people.sort(Comparator.comparingInt(Person::getAge));
        
        // 2. Tri par nom (ordre alphabétique)
        people.sort(Comparator.comparing(Person::getName));
        
        // 3. Tri par âge décroissant
        people.sort(Comparator.comparingInt(Person::getAge).reversed());
        
        // 4. Tri multiple : par ville puis par âge
        people.sort(
            Comparator.comparing(Person::getCity)
                     .thenComparingInt(Person::getAge)
        );
        
        // 5. Tri avec null safety
        List<Person> peopleWithNulls = new ArrayList<>(people);
        peopleWithNulls.add(new Person(null, 40, "Nice"));
        
        peopleWithNulls.sort(
            Comparator.comparing(Person::getName, 
                                Comparator.nullsLast(Comparator.naturalOrder()))
        );
        
        // 6. Tri personnalisé
        people.sort((p1, p2) -> {
            // D'abord par ville
            int cityCompare = p1.getCity().compareTo(p2.getCity());
            if (cityCompare != 0) return cityCompare;
            
            // Puis par âge décroissant
            return Integer.compare(p2.getAge(), p1.getAge());
        });
        
        // 7. Avec les Streams
        List<Person> sorted = people.stream()
            .sorted(Comparator.comparing(Person::getAge))
            .collect(Collectors.toList());
        
        // 8. Trouver le min/max
        Optional<Person> youngest = people.stream()
            .min(Comparator.comparingInt(Person::getAge));
        
        Optional<Person> oldest = people.stream()
            .max(Comparator.comparingInt(Person::getAge));
    }
}
```

---

## Exercices Principaux du Cours

### Exercice 1 : Three Dice (Complet)

```java
public class TripletOfDice {
    private final int sides;
    private int die1, die2, die3;
    private Random random;
    
    /**
     * Crée un triplet de dés.
     * @param sides nombre de faces de chaque dé (doit être > 0)
     */
    public TripletOfDice(int sides) {
        if (sides <= 0) {
            throw new IllegalArgumentException("Le nombre de faces doit être positif");
        }
        this.sides = sides;
        this.random = new Random();
        rollAllDice();
    }
    
    /**
     * Lance tous les dés.
     */
    public void rollAllDice() {
        die1 = random.nextInt(sides) + 1;
        die2 = random.nextInt(sides) + 1;
        die3 = random.nextInt(sides) + 1;
    }
    
    /**
     * Lance un dé spécifique.
     * @param dieNumber numéro du dé (1, 2 ou 3)
     */
    public void rollOneDie(int dieNumber) {
        if (dieNumber < 1 || dieNumber > 3) {
            throw new IllegalArgumentException("Le numéro de dé doit être 1, 2 ou 3");
        }
        
        int newValue = random.nextInt(sides) + 1;
        switch (dieNumber) {
            case 1: die1 = newValue; break;
            case 2: die2 = newValue; break;
            case 3: die3 = newValue; break;
        }
    }
    
    public int getFirstDie() { return die1; }
    public int getSecondDie() { return die2; }
    public int getThirdDie() { return die3; }
    
    public int getTotal() {
        return die1 + die2 + die3;
    }
}

public class DiceUser {
    public static TripletOfDice rollOnce() {
        TripletOfDice triplet = new TripletOfDice(6);
        triplet.rollAllDice();
        return triplet;
    }
    
    public static void main(String[] args) {
        TripletOfDice triplet = rollOnce();
        
        System.out.println("The die number 1 is a " + triplet.getFirstDie());
        System.out.println("The die number 2 is a " + triplet.getSecondDie());
        System.out.println("The die number 3 is a " + triplet.getThirdDie());
        System.out.println("Total: " + triplet.getTotal());
    }
}
```

### Exercice 2 : Car avec Héritage

```java
// Interface de base
public interface Car {
    void drive(double time);
    double getPosition();
    double getSpeed();
}

// Implémentation simple
public class SimpleCar implements Car {
    private double position = 0;
    private double speed = 50; // km/h
    
    @Override
    public void drive(double time) {
        if (time < 0) {
            throw new IllegalArgumentException("Le temps ne peut être négatif");
        }
        position += speed * time;
    }
    
    @Override
    public double getPosition() {
        return position;
    }
    
    @Override
    public double getSpeed() {
        return speed;
    }
}

// Extension avec mémorisation
public class MemorizerCar extends SimpleCar {
    private List<Double> positionHistory = new ArrayList<>();
    
    @Override
    public void drive(double time) {
        super.drive(time); // Réutilise la logique existante
        positionHistory.add(getPosition());
    }
    
    public double getPositionAfterMove(int moveNumber) {
        if (moveNumber < 1 || moveNumber > positionHistory.size()) {
            throw new IndexOutOfBoundsException("Numéro de mouvement invalide");
        }
        return positionHistory.get(moveNumber - 1);
    }
    
    public int getNumberOfMoves() {
        return positionHistory.size();
    }
}
```

### Exercice 3 : Collections - Gestion d'Étudiants

```java
public class Student {
    private final String name;
    private final List<Double> grades;
    
    public Student(String name) {
        this.name = Objects.requireNonNull(name);
        this.grades = new ArrayList<>();
    }
    
    public void addGrade(double grade) {
        if (grade < 0 || grade > 20) {
            throw new IllegalArgumentException("Note doit être entre 0 et 20");
        }
        grades.add(grade);
    }
    
    public double getAverage() {
        return grades.stream()
                     .mapToDouble(Double::doubleValue)
                     .average()
                     .orElse(0.0);
    }
    
    public String getName() { return name; }
    public List<Double> getGrades() { return new ArrayList<>(grades); }
    
    @Override
    public String toString() {
        return String.format("%s (moyenne: %.2f)", name, getAverage());
    }
}

public class StudentManager {
    private Map<String, Student> students = new HashMap<>();
    
    public void addStudent(String name) {
        if (students.containsKey(name)) {
            throw new IllegalArgumentException("Étudiant déjà existant");
        }
        students.put(name, new Student(name));
    }
    
    public void addGrade(String studentName, double grade) {
        Student student = students.get(studentName);
        if (student == null) {
            throw new IllegalArgumentException("Étudiant non trouvé");
        }
        student.addGrade(grade);
    }
    
    public List<Student> getStudentsSortedByAverage() {
        return students.values()
                      .stream()
                      .sorted(Comparator.comparingDouble(Student::getAverage).reversed())
                      .collect(Collectors.toList());
    }
    
    public Optional<Student> getBestStudent() {
        return students.values()
                      .stream()
                      .max(Comparator.comparingDouble(Student::getAverage));
    }
    
    public double getClassAverage() {
        return students.values()
                      .stream()
                      .mapToDouble(Student::getAverage)
                      .average()
                      .orElse(0.0);
    }
}
```

---

## Conseils pour l'Examen

### 1. Révision Stratégique
- **Maîtrisez les bases** : syntaxe, classes, interfaces
- **Comprenez les concepts** : héritage, polymorphisme, généricité
- **Pratiquez les collections** : List, Set, Map et leurs implémentations
- **Connaissez JUnit** : écriture de tests simples

### 2. Patterns de Code Récurrents
- Validation des paramètres avec `Objects.requireNonNull()`
- Contrats documentés avec Javadoc
- Gestion des exceptions appropriées
- Utilisation des collections avec types génériques

### 3. Pièges à Éviter
- Comparaison d'objets avec `==` au lieu de `.equals()`
- Oubli de redéfinir `hashCode()` quand on redéfinit `equals()`
- NullPointerException par manque de validation
- Mauvaise utilisation des exceptions vérifiées vs non vérifiées

### 4. Points d'Attention
- **Encapsulation** : variables privées, getters/setters appropriés
- **Immutabilité** : classes et méthodes défensives
- **Conventions de nommage** : camelCase, PascalCase
- **Documentation** : Javadoc pour les méthodes publiques

---

Cette fiche de révision couvre l'ensemble du programme de votre cours Java L3. Chaque section comprend les concepts théoriques, des exemples pratiques et les exercices principaux. Bonne révision et bonne chance pour votre examen ! 🚀 