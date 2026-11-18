 — Système d'arrosage intelligent

**Livrable 1 : Choix de projet et plan de projet**

## Membres de l'équipe

- Dalia Laifa
- Loic petitpas

## 1. Présentation du projet

### Problématique

L'entretien des plantes nécessite un arrosage régulier et adapté à leurs besoins. Un manque d'eau peut entraîner le dessèchement de la plante, tandis qu'un arrosage excessif peut endommager les racines.

Il n'est pas toujours facile de déterminer quand une plante a besoin d'eau.

### Solution proposée

Notre projet consiste à réaliser un **système d'arrosage intelligent** avec un Arduino Nano ESP32 programmé en MicroPython.

Le système mesurera automatiquement l'humidité de la terre et activera une pompe à eau lorsque celle-ci sera trop sèche.

Il permettra également de surveiller le niveau d'eau du réservoir et d'afficher l'état de la plante.

### Fonctionnalités prévues

- Mesurer l'humidité de la terre à l'aide d'un capteur analogique.
- Déterminer si la terre est sèche selon un seuil calibré.
- Activer automatiquement une pompe lorsque la plante a besoin d'eau.
- Arrêter la pompe après une durée d'arrosage limitée.
- Vérifier le niveau d'eau du réservoir.
- Afficher les informations sur un écran OLED.
- Indiquer l'état du système avec une DEL RGB.
- Permettre de changer de mode avec un bouton.
- Détecter certaines erreurs et empêcher un arrosage dangereux.

## 2. Modes de fonctionnement

### Mode automatique

L'Arduino surveille l'humidité de la terre.

Lorsque la valeur mesurée indique que la terre est trop sèche, le système vérifie que le réservoir contient de l'eau.

Si toutes les conditions sont respectées, la pompe s'active pendant une courte durée.

Après l'arrosage, le système attend que l'eau se répartisse dans la terre avant de reprendre une décision.

### Mode pause

Le système continue à mesurer l'humidité et à afficher son état, mais la pompe ne peut pas s'activer.

Un bouton permet de passer d'un mode à l'autre.

## 3. Diagramme fonctionnel

```text
       CAPTEUR D'HUMIDITÉ
           (Analogique)
                |
                v
      +-------------------+
      |                   |
      |   Arduino Nano    |
      |       ESP32       |
      |                   |
      +-------------------+
          ^           ^
          |           |
   CAPTEUR NIVEAU   BOUTON
       D'EAU        DE MODE
   (Numérique)     (Numérique)

                |
                v
      ANALYSE DES DONNÉES
      - Humidité du sol
      - Niveau d'eau
      - Mode sélectionné
      - Conditions de sécurité
                |
                v
      +-------------------+
      |     SORTIES       |
      +-------------------+
       |       |        |
       v       v        v
     POMPE   DEL RGB   OLED
     À EAU    (PWM)   (I2C)
```

## 4. Matériel prévu

| Quantité | Composant | Fonction |
|---|---|---|
| 1 | Arduino Nano ESP32 | Contrôleur principal |
| 1 | Capteur d'humidité du sol analogique | Mesurer l'humidité |
| 1 | Capteur numérique de niveau d'eau | Vérifier le réservoir |
| 1 | Mini-pompe à eau | Arroser la plante |
| 1 | Module de commande de pompe | Commander la pompe |
| 1 | DEL RGB et résistances | Indiquer l'état du système |
| 1 | Écran OLED I2C | Afficher les informations |
| 1 | Bouton-poussoir | Changer de mode |
| 1 | Breadboard | Réaliser le circuit |
| Plusieurs | Fils de connexion | Relier les composants |
| Selon besoins | Alimentation adaptée | Alimenter le système |

**Remarque :** Cette liste est préliminaire. Les composants devront être confirmés avec l'inventaire disponible à l'école.

### Composant nouveau

Nous prévoyons intégrer un écran OLED utilisant le protocole I2C.

Cet écran permettra d'afficher l'humidité de la terre, le mode sélectionné et les messages d'erreur.

Son admissibilité comme composant nouveau devra être validée avec le professeur.

## 5. Attribution préliminaire des broches

| Broche Arduino | Composant | Type |
|---|---|---|
| A0 | Capteur d'humidité du sol | Entrée analogique |
| D2 | Capteur de niveau d'eau | Entrée numérique |
| D3 | Bouton-poussoir | Entrée numérique |
| D5 | DEL RGB | Sortie PWM |
| D6 | Commande de la pompe | Sortie numérique |
| A4 | OLED — SDA | I2C |
| A5 | OLED — SCL | I2C |

Les broches sont proposées à titre préliminaire et seront vérifiées lors du montage.

La pompe sera alimentée par une alimentation adaptée et commandée par un module approprié, sans être branchée directement à une sortie de l'ESP32.

## 6. Logique de fonctionnement

1. Initialiser les capteurs et les sorties.
2. Lire l'humidité de la terre.
3. Lire le niveau d'eau du réservoir.
4. Vérifier le mode sélectionné.
5. Si le mode automatique est actif et que la terre est sèche, vérifier les conditions de sécurité.
6. Activer la pompe pendant une durée limitée si le réservoir contient de l'eau.
7. Arrêter la pompe et attendre la stabilisation de l'humidité.
8. Mettre à jour l'affichage OLED et la DEL RGB.
9. Répéter les mesures sans bloquer les autres tâches.

### Calibration

Les mesures du capteur d'humidité seront comparées à des valeurs obtenues dans une terre sèche et une terre humide.

Le seuil de déclenchement sera déterminé expérimentalement afin d'éviter un arrosage inutile.

Les valeurs exactes seront ajoutées après les premiers tests.

## 7. Gestion des erreurs

Le système devra prévoir les situations suivantes :

- **Réservoir vide :** la pompe reste désactivée et un avertissement est affiché.
- **Mesure d'humidité invalide :** l'arrosage est désactivé.
- **Temps maximal d'arrosage atteint :** la pompe s'arrête automatiquement.
- **Composant absent ou défectueux :** le système signale le problème sans interrompre inutilement les autres fonctions.

Les temporisations seront réalisées avec `ticks_ms()` et `ticks_diff()` pour éviter les longues pauses bloquantes.

## 8. Répartition du travail

### Dalia Laifa

- Lecture du capteur d'humidité.
- Calibration des mesures.
- Programmation de l'arrosage automatique.
- Gestion des modes de fonctionnement.
- Participation aux tests.

### Deuxième membre

- Lecture du capteur de niveau d'eau.
- Commande de la pompe.
- Programmation de la DEL RGB.
- Intégration de l'écran OLED.
- Participation aux tests.

### Travail commun

- Montage et vérification du circuit.
- Intégration des différents composants.
- Gestion des erreurs.
- Tests du système complet.
- Documentation du projet.
- Démonstration finale.

Chaque membre effectuera régulièrement des commits et des push sur GitHub et devra comprendre l'ensemble du programme.

## 9. Planification

| Période | Travail prévu |
|---|---|
| 23 au 27 novembre | Choix du projet et plan initial |
| 30 novembre au 4 décembre | Tests individuels des capteurs et des sorties |
| 7 au 10 décembre | Intégration, modes, tests et stabilisation |
| 10 décembre | Remise finale |
| Semaine du 14 décembre | Examen oral individuel |

## 10. Utilisation de l'intelligence artificielle

ChatGPT a été utilisé pour aider à structurer le projet, préparer le plan initial, proposer un diagramme fonctionnel et organiser la documentation.

Les membres de l'équipe vérifieront les informations, les branchements et le fonctionnement du code avant la remise.

## 11. État d'avancement

- [x] Choix du thème
- [x] Définition de la problématique
- [x] Description des fonctionnalités
- [x] Diagramme fonctionnel préliminaire
- [x] Liste préliminaire du matériel
- [x] Attribution préliminaire des broches
- [x] Répartition initiale du travail
- [ ] Vérification du matériel disponible
- [ ] Validation du projet par le professeur
- [ ] Ajout du deuxième membre et du lien GitHub
- [ ] Montage et tests des composants
