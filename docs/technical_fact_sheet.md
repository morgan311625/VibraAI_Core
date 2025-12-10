FICHE TECHNIQUE PROFESSIONNELLE — VibraAI_Core v1

Format : une page, factuelle, sans marketing inutile, 100 % conforme aux documents internes.

VibraAI_Core — Offline Embedded AI Engine (C / MCU / Linux)

Fiche technique

 1. Objet

VibraAI_Core est un moteur d’IA embarquée offline, écrit en C pur, conçu pour tourner sur microcontrôleurs ARM Cortex-M et plateformes Linux embarquées.
Il transforme une fenêtre de 2 s de données accéléromètre + vitesse en trois scores déterministes.

Aucune dépendance externe. Aucun malloc. Temps d’inférence < 1 ms.

 2. Entrées moteur

Fenêtre glissante de 2 secondes contenant :

Accéléromètre : 1–3 axes, 50–200 Hz

Vitesse moyenne (GPS / CAN)

Timestamp (ms)

Format interne : VibraAccelSample[N] + avg_speed + ts.

 3. Pipeline interne (résumé)

FeatureExtractor

mean, rms, var, max_abs (3 axes)

jerk_rms, jerk_max (3 axes)

low_band_energy, high_band_energy

speed, inv_speed_clamped
→ vecteur 20–30 scalaires

Normalizer

min/max issus de scales.json

clamp strict dans [0,1]

CoreHead (MLP quantifié int8)

modèle quelques KB

accumulateurs int32

inference < 1 ms

PostProcessing

clamps

règles statiques (rules.json)

sortie déterministe

 4. Sorties moteur

Toutes les 2 secondes, le moteur retourne :

Nom	Domaine	Description
driver_score	0–100	stabilité du comportement dynamique
vehicle_anomaly	0–1	détection d’anomalie véhicule
road_quality	0–1	qualité de route (lisse → dégradée)

Ces sorties sont reproductibles, fixes, offline.

 5. Performances

Inference : < 1 ms (CPU), 2–5 ms (MCU 80–120 MHz)

RAM : < 32 KB

Blob modèle : < 100 KB

Lib C : ~100–150 KB

Aucun malloc / aucune dépendance externe

 6. API publique (SDK C)

Le SDK expose un header unique :

int vibra_init(VibraContext*, const VibraConfig*);
int vibra_learn_window(VibraContext*, const VibraAccelSample*, float avg_speed, long long ts);
int vibra_infer_window(VibraContext*, const VibraAccelSample*, float avg_speed, long long ts, VibraMetrics*);
int vibra_save_state(VibraContext*, unsigned char*, int);
int vibra_load_state(VibraContext*, const unsigned char*, int);


Aucune autre fonction n’est nécessaire ni autorisée.

 7. Contenu du SDK
sdk/
  include/vibra_core.h
  lib/libvibra_core.a
  lib/model_blob.bin
  config/{scales.json, profile.json, rules.json}
  examples/example_inference.c
  docs/README_INTEGRATION.pdf
  docs/CHANGELOG.txt

 8. Intégration (résumé MCU/Linux)

Charger blob + JSON via vibra_init()

Bufferiser 2 s de samples

Appeler vibra_infer_window()

Consommer les scores

Aucune configuration dynamique. Aucun cloud requis.

 9. Cas d’usage typiques

télématique / fleet management

boîtiers embarqués

analyse route/vehicle offline

produits MCU nécessitant un moteur IA “sans Python/sans framework”

FIN
