# Super-résolution via CNN

### Prérequis

Tout d'abord, une carte graphique <b>Nvidia</b> (ni AMD, ni Intel intégrée) est vivement conseillée pour paralléliser le CNN.
Il faudra alors installer CUDA. Pas de CUDA = des dizaines de fois plus lent.<br>

À installer :
- [Python 3.x](https://www.python.org/downloads/)
- [PyTorch](https://pytorch.org)
- [TorchVision](https://pytorch.org) (intégré à PyTorch)
- [CUDA](https://developer.nvidia.com/cuda-downloads) (facultatif pour la résolution)
- PIL (intégré à Python)
- NumPy (intégré à Python)

Pour générer l'image cible (via le .sh) :
- [GTVImageVect](https://github.com/kerautret/GTVimageVect) (C++ à compiler ou binaire Linux à télécharger)
- [ImageMagick](https://imagemagick.org/script/download.php) (Disponible via `sudo apt install imagemagick`)
- De manière générale, un Linux, un émulateur
ou [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10),
le noyau Linux intégré officiellement à Windows 10

<hr>

### Entrainement
```
Entrainement: train.py

Arguments:
  -h, --help                            Message d'aide
  -u, --upscaleFactor     ! REQUIS      Facteur de super-résolution
  --batchSize               16          Taille du batch d'entrainement
  --testBatchSize           10          Taille du batch de test
  --nbEpochs                50          Nombre de simulations max. 0 pour désactiver la limite
  --learningRate            0.01        Taux d'apprentissage
  --cpu                                 Utilise le CPU et non le GPU / CUDA
  --nbThreads               4           Nombre de threads pour le data loader
  --seed                                Seed utilisée pour l'aléatoire
  --noiseLimit              25          Peak signal-to-noise ratio (PSNR), détermine la précision du modèle en dB, 0 = pas de limite. PSNR élevé = modèle précis
  -q, --quiet                           Silencieux
```

Ce programme générera des fichiers "model_epoch_`n`.pth" correspondant au modèle à l'epoch `n`,
dans un dossier saved_model_u`t`_bs`bs`_tbs`tbs`_lr`lr`,
où `t` correspond au facteur d'échelle,
`bs` à la taille du batch d'entrainement,
`tbs` la taille du batch de test
et `lr` au taux d'apprentissage.

Les images low res doivent être situées dans un dossier "dataset/input",
et les cibles high res dans un dossier "dataset/target",
où chaque image de qualité différente a le même nom dans les 2 dossiers.

Si le programme est trop gourmand en RAM ou en VRAM, vous pouvez régler les options --batchSize et --testBatchSize.

#### Exemple :
`python train.py --upscaleFactor 4 --nbEpochs 20`

<hr>

### Super-résolution
```
Super-résolution: super_resolve.py

Arguments:
  -h, --help                            Message d'aide
  -i, --inputPath         ! REQUIS      Image d'entrée
  -m, --modelPath         ! REQUIS      Modèle .pth à utiliser
  -o, --outputPath          out.png     Image de sortie
      --cpu                             Utilise le CPU et non le GPU / CUDA
  -q, --quiet                           Silencieux
```

#### Exemple :
`python super_resolve.py --inputPath pomme.png --modelPath model.pth --outputPath pomme_x4.png`

<hr>

### Licence

GPLv3

<hr>

### Crédits

Inspiré de [<u>cet article</u>](https://arxiv.org/abs/1609.05158)
<br>
Code de base disponible [<u>ici</u>](https://github.com/pytorch/examples/tree/master/super_resolution)
<br>
Dataset cible construit à partir de l'algorithme [<u>GTVimageVect</u>](https://github.com/kerautret/GTVimageVect)
<br>
Sujet disponible [<u>ici</u>](https://www.lama.univ-savoie.fr/mediawiki/index.php/VISI401_CMI_:_bibliographie_scientifique#Algorithmes_de_super-r.C3.A9solution_par_apprentissage)

[//]: # ("Real-Time Single Image and Video Super-Resolution Using an Efficient Sub-Pixel Convolutional Neural Network" - Shi et al.)

<hr>

Romain THEODET, CMI INFO L2 USMB, 2019/2020
