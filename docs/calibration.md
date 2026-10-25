# Calibration du Vatten-Pebble

Chaque capteur capacitif v1.2 a sa propre plage de tension. Calibrer chaque
Pebble individuellement, **une fois monté et scellé** : le boîtier et le
silicone peuvent décaler légèrement les valeurs.

## Matériel

- Vatten-Hub flashé et connecté (voir README)
- Un verre d'eau
- Le terreau habituel de la plante, sec puis arrosé à saturation

## 1. Relever la tension à sec (`dry_v`)

1. Pebble propre et sec, lame **à l'air libre**.
2. Dans Home Assistant (ou les logs ESPHome), lire l'entité
   « Plante N - Tension capteur ».
3. Attendre que la valeur se stabilise (≈ 30 s), la noter : c'est `dry_v`.
   Ordre de grandeur : 2,6 à 3,0 V.

## 2. Relever la tension dans l'eau (`wet_v`)

1. Plonger la lame dans le verre d'eau **jusqu'à la ligne de repère du
   capteur, pas au-delà** : l'électronique ne doit jamais être immergée.
2. Attendre la stabilisation, noter la valeur : c'est `wet_v`.
   Ordre de grandeur : 1,0 à 1,4 V.

## 3. Renseigner les valeurs

Dans `esphome/vatten-hub.yaml`, pour le canal concerné :

```yaml
dry_v: "2.80"   # votre valeur à sec
wet_v: "1.20"   # votre valeur dans l'eau
```

Reflasher, puis vérifier : à l'air, l'humidité doit afficher ~0 %, dans
l'eau ~100 %.

## 4. Vérifier en conditions réelles

1. Planter le Pebble (lame de 60 à 65 mm dans le terreau) dans un terreau sec.
2. Arroser à saturation, laisser s'égoutter, noter le % obtenu.
3. Fixer le **seuil d'arrosage** entre « terreau sec » et « terreau juste
   humide » (point de départ : 30 %, à affiner selon la plante).

## 5. Mesurer le débit de la pompe

1. Placer la sortie du Vatten-Totem au-dessus d'un verre doseur.
2. Presser « Arroser maintenant » : noter le volume pour une impulsion.
3. Ajuster la **durée d'impulsion** (10 à 15 s recommandé) et le **lockout**
   (30 à 60 min) pour que le volume reste modéré et que l'eau ait le temps
   d'atteindre le capteur avant la mesure suivante.

## Dépannage

| Symptôme | Piste |
|---|---|
| Valeurs qui sautent | Câble trop long ou non blindé, masse commune manquante |
| Toujours 0 % ou 100 % | `dry_v` / `wet_v` inversés ou broche ADC2 utilisée |
| Pompe qui se redéclenche trop vite | Augmenter le lockout, vérifier le temps de diffusion de l'eau |
| Lecture qui dérive avec le temps | Humidité dans la cavité : refaire le scellement silicone |
