# Projet de Traitement et Extraction de Données à partir de PDFs avec OCR

Ce projet permet de convertir des fichiers PDF en images, définir des structures de zones à analyser, réaliser l'extraction de texte via OCR, et visualiser ou sauvegarder les résultats. Il s'appuie sur plusieurs bibliothèques Python pour la manipulation d'images, l'OCR, et la gestion de données.

### **Fonctionnalités principales**

- Conversion de PDF en images (pages).

- Définition et sauvegarde de structures de zones ("layout") pour l'extraction ciblée.

- Visualisation des zones de detection sur les images.

- Extraction automatique de texte par OCR dans ces zones.

- Enregistrement des données extraites dans des fichiers JSON.

- Gestion de structures différentes selon les besoins (ex. structures pour différents formulaires).

### **Technologies utilisées**

- pdf2image: Conversion des PDFs en images.

- Matplotlib: Visualisation des images et délimitations.

- Pillow, fork de PIL: Manipulation d’images et recadrage.

- PyTesseract: Extraction de texte par OCR.

- json: Gestion des fichiers de configuration et résultats.

### **Fonctionnement**

1. **Chargement du PDF**

Le script charge un fichier PDF et le convertit en une liste d'images de pages.

2. **Définition de la structure**

On définit une structure ("layout") précisant les zones d'intérêt (coordonnées, couleurs, labels).

3. **Visualisation**

La zone de chaque structure peut être visualisée sur l’image avec des rectangles superposés.

4. **Extraction de texte via OCR**

Pour chaque zone, le script effectue une coupe (crop) sur l’image, puis applique Tesseract pour extraire le texte.

5. **Enregistrement des résultats**

Les textes extraits sont stockés dans des fichiers JSON structurés pour traitement ultérieur.

### **Prérequis**

- Python 3.x

- Tesseract-OCR

- Librairies Python :

pip install pdf2image matplotlib pillow pytesseract pandas

### **Script principal**

1. **Charger le PDF**

from pdf2image import convert_from_path
images = convert_from_path('chemin/vers/votre.pdf')

2. **Définir une structure**
```python
struct = [
{
'boxes': [
{'x': 80, 'y': 2370, 'w': 370, 'h': 130, 'c': 'r', 'col': 'Date'},
# autres zones...
],
'decal': 385
}
]
```

3. **Visualiser une zone**
```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle

page_num = 2
image = images[page_num - 1]
group = struct[0]['boxes']

fig, ax = plt.subplots(figsize=(10,10))
ax.imshow(image)
for box in group:
rect = Rectangle((box['x'], box['y']), box['w'], box['h'], linewidth=1, edgecolor=box['c'], facecolor='none')
ax.add_patch(rect)
plt.show()
```

4. **Extraction OCR dans une zone**
```python
from PIL import Image
import pytesseract

cropped = image.crop((box['x'], box['y'], box['x'] + box['w'], box['y'] + box['h']))
text = pytesseract.image_to_string(cropped)
print(f"{box['col']} : {text}")
```

5. **Sauvegarder les résultats**
```python
import json
data = [{'col': box['col'], 'text': text} for box in group]
with open('resultats.json', 'w') as f:
json.dump(data, f, ensure_ascii=False, indent=4)
```

### **Structure du projet**

- pdfs/ : Dossier contenant les fichiers PDF

- structures/ : Dossier de sauvegarde des structures JSON

- dataout/ : Dossier des fichiers JSON de sortie

- main.py : Script principal intégrant toutes les étapes

### **Personnalisation**

- Modifier les paramètres dans la structure pour s’adapter à différents documents.

- Ajuster les coordonnées des zones selon les documents spécifiques.

- Ajouter ou modifier des processus pour automatiser le traitement de plusieurs pages.