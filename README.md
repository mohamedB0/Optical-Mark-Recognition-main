# AutoGrader

## Description
AutoGrader est une application pour la correction automatique des tests à choix multiples. Elle utilise la vision par ordinateur pour détecter, analyser et noter des feuilles de test remplies à la main. L'application peut utiliser une webcam en temps réel ou une image téléchargée.

## Fonctionnalités
- Interface graphique conviviale
- Capture via webcam ou importation d'images
- Détection automatique des contours de la feuille de test
- Analyse des réponses marquées
- Comparaison avec les réponses correctes
- Affichage du score en pourcentage
- Visualisation des réponses correctes/incorrectes

## Prérequis
- Python 3.6+
- OpenCV
- NumPy
- Tkinter
- keyboard

## Installation
1. Clonez ce dépôt
2. Installez les dépendances:
```
pip install opencv-python numpy keyboard
```

## Utilisation
Exécutez le fichier principal:
```
python AutoGrader.py
```

## Adaptation à un nouveau modèle de test

Pour utiliser AutoGrader avec un modèle de test différent, vous devrez effectuer les modifications suivantes:

### 1. Modifier le nombre de questions et d'options
Dans `AutoGrader.py`, mettez à jour les variables suivantes:
```python
self.questions = n  # Remplacer par le nombre de questions de votre modèle
self.choices = m    # Remplacer par le nombre d'options par question
```

### 2. Mettre à jour les réponses correctes
```python
self.ans = [1, 2, 3, 4, 5,....,]  # Remplacer par les indices des bonnes réponses jusqu'à n-1 (0 = première option)
```

### 3. Adapter la fonction de découpage des cases
Dans `utils.py`, modifiez la fonction `splitBoxes`:
```python
def splitBoxes(img):
    rows = np.vsplit(img, 5)  # Remplacer 5 par le nombre de questions
    boxes = []
    for r in rows:
        cols = np.hsplit(r, 5)  # Remplacer 5 par le nombre d'options
        for box in cols:
            boxes.append(box)
    return boxes
```

### 4. Ajuster les paramètres de grille
Mettez à jour les fonctions `drawGrid` et `showAnswers` dans `utils.py` si votre disposition est différente.

### 5. Modifier les points de transformation
Si la zone de notation a une taille ou position différente:
```python
ptsG2 = np.float32([[0, 0], [325, 0], [0, 150], [325, 150]])  # Ajuster selon vos besoins
```

### 6. Ajuster les seuils de détection
Si votre modèle utilise un fond ou des marquages différents:
```python
imgThresh = cv2.threshold(imgWarpGray, 170, 255, cv2.THRESH_BINARY_INV)[1]  # Ajuster 170 si nécessaire
```

### 7. Personnaliser l'interface utilisateur
Modifiez les textes et les éléments de l'interface selon vos besoins.

## Conseils pour la création d'un modèle de test compatible
1. Utilisez un papier de couleur claire (blanc de préférence)
2. Créez un contour rectangulaire bien marqué autour de la zone de test
3. Créez une zone distincte pour le score (également rectangulaire)
4. Disposez les questions et options dans une grille régulière
5. Utilisez des cases ou cercles bien définis pour marquer les réponses
6. Assurez un bon contraste entre le fond et les marquages

## Débogage
- Utilisez la fenêtre "Result" pour visualiser les différentes étapes de traitement
- Appuyez sur 's' pour sauvegarder l'image traitée
- Appuyez sur 'q' pour quitter l'application
- Si la détection échoue, vérifiez l'éclairage et le contraste de votre feuille

## Structure du projet
- `AutoGrader.py`: Application principale et interface utilisateur
- `utils.py`: Fonctions utilitaires pour le traitement d'image
- `Scanned/`: Dossier où les images traitées sont sauvegardées

## Licence
[MIT]
