**Compression des données - Cours 3**

[TOC]

# Introduction
## Rappels
Apprendre théorème de l'échantillonnage

La compression non-conservative est utilisée pour le son et les images. Sur les images, elle floute (gaussian) / mixe certains pixels. Les signaux numériques ne se retrouve pas dans la nature (analogique) on a donc affaire à une conversion analogique -> numérique et numérique -> analogique.
Pour le partiel: on doit savoir ce qu'est la _fréquence de Nyquik_.

## Vocabulaire
**Fort gradient**: beaucoup de pixel différent les uns à côtés des autres.
**Haute fréquence (HF)** (spatiale): idem, vérifier sur internet.
**Basse fréquence**: différence entre pixels qui sont lointains. Je ne veux pas y toucher, elle structure l'image.
**Spectre**: décomposition de la fréquence d'un signal.

## Hors sujet
La musique c'est une harmonie de son, ce n'est pas un son pur.

# Compression d'images (dont JPG)
## Objectif
Cherche à conserver le maximum de détails sans pour autant enregistrer ceux qui sont trop minimes. On va donc chercher à garder les HF utiles et se passer de celles qui sont futiles. Il faut se rappeler que les images ont très peu de redondance contrairement au texte d'où ce processus minutieux. 

## Outils et Principes
Le dispositif qu'on va utiliser est la **transformée de Fourier** _(1822)_ car il va nous fournir le tableau de fréquence qu'on pourrait filtrer derrière et donc compresser l'image originale. Pour retrouver l'image, on fait la transformée inverse.
**Principe de superposition**:  _(exemple des canards)_ Un phénomène ondulatoire compliqué.

_JPG va jusqu'à rho de 8._

Formule de série de Fourier:
N: longueur de la corde
phi(N): le déphasage 
X: l'endroit de la corde où l'on se met
2pi/N: convertisseur d'unité (distance en radian)

**Théorème des séries de Fourier**:
Une fonction f(x) **périodique** peut s'exprimer de manière **unique** comme une somme de fonctions circulaires (trigonométriques sin/cos/...) de fréquence multiple de la fréquence initiale chacune étant affectée d'une amplitude et d'une phase propre.

Formule de base (source: internet)
$$F_n(x) = a_0 + \sum_{k= 1}^{k=n} \Big(a_k\cos(kx) + b_k\sin(kx)\Big)$$

Pour une Longueur L (lors du cours, t = 2):
$$f(t) \sim a_0 + \sum_{n=1}^{\infty} \left(a_n\cos\left(n\frac{\pi t}{L}\right) + b_n\sin\left(n\frac{\pi t}{L}\right)\right)$$

Exemple, pour:
$$ f(x) = {2,3,4,4}$$ $$ M = 4$$ $$C_n = \frac{1}{4} \sum_{x=0}^{3} f(x) \exp({-i \frac{\pi}{2} + x})$$

$$C_0 = \frac{1}{4} (2 + 3 + 4 +4) => \rho_0 = \frac{11}{4} $$
$$C_1 = \frac{1}{4} (2 * e^0 + 3 * e^{-i \frac{\pi}{2}}+ 4 * e ^{-i \pi} + 4 * e ^{-i \frac{3\pi}{2}})$$

On utilise la formule d'Euler:
$$\mathrm{e}^{\mathrm{i}\,x} = \cos(x) + \mathrm{i}\,\sin(x)"$$

Ce qui donne:
$$C_1 = \frac{1}{4} (2 - 3i - 4 + 4i) => \rho_1 = \frac{1}{4} (-2 + i) $$ $$\rho_1= \sqrt{\frac{1}{4} + 1} = \frac{\sqrt{5}}{2}$$

## JPEG
### Qu'est-ce?
JPEG = Join Photography Expert Group. Sentant l'arrivée de nouvelles technologies: un groupe d'expert de la photographie se réunit pour mettre en place une nouvelle norme.

### Principe
1. **Subdivise l'image en carrés 8x8** (2^3) car on retouche les hautes fréquences donc inutile de regarder toute l'image d'un coup.
2. **FFT** (Fast Fourier Transform): Transformée de Fourier Rapide fonctionne bien avec les puissances de 2.